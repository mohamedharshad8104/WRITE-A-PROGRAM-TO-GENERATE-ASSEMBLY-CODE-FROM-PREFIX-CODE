# WRITE-A-PROGRAM-TO-GENERATE-ASSEMBLY-CODE-FROM-PREFIX-CODE
The primary objective of this project is to develop a program that takes expressions in prefix notation as input and produces corresponding assembly language code as output.
#python
def generate_assembly(prefix_expression):
    stack = []

    # Define a dictionary for mapping operators to assembly instructions
    operators = {
        '+': 'add',
        '-': 'sub',
        '*': 'imul',
        '/': 'idiv',
    }

    assembly_code = []

    # Iterate through the tokens in reverse order (right to left)
    for token in reversed(prefix_expression):
        if token.isdigit():
            stack.append(token)  # Push operands onto the stack
        elif token in operators:
            # Pop two operands from the stack
            operand1 = stack.pop()
            operand2 = stack.pop()

            # Generate the corresponding assembly instruction
            instruction = operators[token]

            # Emit the assembly instruction
            assembly_code.append(f'{instruction} {operand1}, {operand2}')

            # Push the result back onto the stack
            stack.append(operand1)
        else:
            raise ValueError(f"Invalid token: {token}")

    # The final result should be on top of the stack
    if len(stack) != 1:
        raise ValueError("Invalid prefix expression")

    # Add code to print the result (assuming x86 assembly)
    result = stack[0]
    assembly_code.append(f'mov eax, {result}')
    assembly_code.append('call print_result')

    return '\n'.join(assembly_code)

# Get the prefix expression from the user
prefix_expression = input("Enter a prefix expression: ").split()

# Example usage:
assembly_code = generate_assembly(prefix_expression)
print(assembly_code)
