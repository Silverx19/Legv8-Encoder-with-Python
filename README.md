# Legv8-Encoder-with-Python
project i have where i need to input Legv8 instructions
This is my code
# Define dictionaries for opcode and function mappings
opcodes = {
    "ADD": "10001011000",
    "SUB": "11001011000",
    "AND": "10001010000",
    "ORR": "10101010000",
    "LDUR": "11111000010",
    "STUR": "11111000000",
    "ADDI": "1001000100",
    "SUBI": "1101000100"
}

functions = {
    "ADD": "10001011001",
    "SUB": "11001011010",
    "AND": "10001010001",
    "ORR": "10101010001"
}

# Function to convert binary to hexadecimal
def binary_to_hex(binary):
    hex_value = hex(int(binary, 2))[2:].upper()
    return hex_value.zfill(8)

# Read input file name from user
input_file_name = input("Enter input file name: ")

# Read output file name from user
output_file_name = input("Enter output file name: ")

# Open input file for reading
with open(input_file_name, "r") as input_file:
    # Open output file for writing
    with open(output_file_name, "w") as output_file:
        # Loop through each line in input file
        for line in input_file:
            # Remove leading/trailing whitespaces and split line by space
            parts = line.strip().split(" ")

            # Extract instruction components
            instruction = parts[0]
            rd = parts[1]
            rs1 = parts[2]

            # Check if instruction is R-type
            if instruction in functions:
                rs2 = parts[3]
                # Assemble machine code for R-type instruction
                opcode = opcodes[instruction]
                funct3 = functions[instruction]
                machine_code = opcode + rs2 + rs1 + funct3 + rd + "0110011"
                # Convert machine code to hexadecimal
                hex_code = binary_to_hex(machine_code)
                # Write instruction, machine code, and hexadecimal code to output file
                output_file.write(f"{instruction} {rd}, {rs1}, {rs2}\t{machine_code}\t{hex_code}\n")
            # Check if instruction is D-type
            elif instruction == "LDUR" or instruction == "STUR":
                offset = parts[3]
                # Assemble machine code for D-type instruction
                opcode = opcodes[instruction]
                machine_code = opcode + offset + rs1 + rd + "0110011"
                # Convert machine code to hexadecimal
                hex_code = binary_to_hex(machine_code)
                # Write instruction, machine code, and hexadecimal code to output file
                output_file.write(f"{instruction} {rd}, [{rs1}, #{offset}]\t{machine_code}\t{hex_code}\n")
            # Check if instruction is I-type
            elif instruction == "ADDI" or instruction == "SUBI":
                imm = parts[3]
                # Assemble machine code for I-type instruction
                opcode = opcodes[instruction]
                machine_code = opcode + imm + rs1 + rd + "0010011"
                # Convert machine code to hexadecimal
                hex_code = binary_to_hex(machine_code)
                # Write instruction, machine code, and hexadecimal code to output file
                output_file.write(f"{instruction} {rd}, {rs1}, #{imm}\t{machine_code}\t{hex_code}\n")

My issue is that i cant get my input file to take LEgv8 insturctions
