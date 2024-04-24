code_path = "FilePath/code.txt"     #add r like r"path" if it gives path error
macro_path = "FilePath/macro.txt"   #add r like r"path" if it gives path error

# Error handling for macro file
try:
    with open(macro_path, "r") as macro_file:
        macrol = [line.rstrip('\n').upper() for line in macro_file]
        if 'MACRO' in macrol:
            macroname = macrol[macrol.index('MACRO') + 1]
            macrol.remove(macroname)
            macrol.remove('MACRO')
            if 'MEND' in macrol:
                macrol.remove('MEND')
            else:
                raise ValueError("'MEND' not found in MACRO definition.")
            macroname = macroname.split()[0]
        else:
            raise ValueError("'MACRO' not found in MACRO definition.")
    
    # Define function for macro invocation
    def callMacro(arg, macrol):
        temp = []
        for i in macrol:
            temp.append(i.replace('&ARG', arg))
        return tuple(temp)

except IOError:
    print("Error: File not found at path: {}".format(macro_path))
    exit()
except ValueError as ve:
    print("Error: {}".format(ve))
    exit()

# Error handling for code file
try:
    with open(code_path, "r") as code_file:
        codel = [line.rstrip('\n') for line in code_file]
        codelen = len(codel)
        argl = []
        macrocount = 0
        
        # Iterate through code lines
        for i in range(len(codel)):
            a = codel[i].split()
            if a and a[0] == macroname:
                codel[i : i + len(macrol) - 2] = callMacro(a[1], macrol)
                argl.append(a[1])
                macrocount += 1
        
        # Output the evaluated program
        print("\nEvaluated Program: ")
        for i in codel:
            print(i)
        
        # Output statistical information
        print("\n\nStatistical Output")
        print("Number of instructions in input source code (excluding Macro calls) = {}".format(codelen - macrocount))
        print("Number of Macro calls = {}".format(macrocount))
        print("Number of instructions defined in the Macro call = {}".format(len(macrol)))
        for i, data in enumerate(argl):
            print("Actual argument during Macro call {} = {}".format(i + 1, data))
        print("Total number of instructions in the expanded source code = {}".format(len(codel)))

except IOError:
    print("Error: File not found at path: {}".format(code_path))
