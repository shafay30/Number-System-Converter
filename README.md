# Number-System-Converter
"""
This python program is a utility for converting between binary, decimal, 
hexadecimal, and octal number systems. It offers a menu-driven interface where 
users can select the type of conversion they want to perform and enter the 
value they want to convert. The program then displays the conversion results 
on the console and writes them to a file called cps109_a1_output.txt.

This utility aims to support students enrolled in Computer Organization I (CPS213) 
at TMU by reinforcing understanding of binary concepts. It provides functionalities 
for converting between different number systems (binary, decimal, hexadecimal, octal) 
to solidify the understanding of number representations, arithmetic operations, 
and data conversions are fundamental to the course and computer science as a whole. 
By allowing students to input values in one number system and view the 
corresponding representations in others, this program serves as a practical tool 
for illustrating connections between binary arithmetic, data representation, and 
the broader concepts covered in CPS213. Integrating explanations or hints related 
to binary conversions within the program could further supplement the learning 
process and solidify students' grasp of these foundational concepts essential to 
computer science.
"""

# Function to convert binary to decimal
def binary_to_decimal(binary):
    # Convert binary to decimal using a reverse iteration method
    decimal = 0
    power = 0
    for digit in reversed(binary):
        decimal += int(digit) * (2 ** power)
        power += 1
    return decimal

# Function to convert decimal to binary
def decimal_to_binary(decimal):
    # Convert decimal to binary using Python's built-in bin() function
    binary = bin(decimal)[2:]
    return binary

# Function to convert hexadecimal to binary
def hexadecimal_to_binary(hexadecimal):
    # Convert hexadecimal to decimal, then to binary
    decimal = int(hexadecimal, 16)
    binary = bin(decimal)[2:]
    return binary

# Function to convert octal to binary
def octal_to_binary(octal):
    # Convert octal to decimal, then to binary
    decimal = int(octal, 8)
    binary = bin(decimal)[2:]
    return binary

# Function to convert binary to hexadecimal
def binary_to_hexadecimal(binary):
    # Convert binary to decimal, then to hexadecimal
    decimal = binary_to_decimal(binary)
    hexadecimal = hex(decimal).upper()[2:]
    return hexadecimal

# Function to convert binary to octal
def binary_to_octal(binary):
    # Convert binary to decimal, then to octal
    decimal = binary_to_decimal(binary)
    octal = oct(decimal)[2:]
    return octal

# Function to display conversion results to a file with proper formatting
def display_results_to_file(input_value, conversion_type, file):
    # Display results in the specified format based on the conversion type
    if conversion_type == 'binary':
        file.write(f"Binary: {input_value}\n")
        file.write(f"Decimal: {binary_to_decimal(input_value)}\n")
        file.write(f"Hexadecimal: {binary_to_hexadecimal(input_value)}\n")
        file.write(f"Octal: {binary_to_octal(input_value)}\n")
    elif conversion_type == 'decimal':
        input_value = int(input_value)
        file.write(f"Decimal: {input_value}\n")
        file.write(f"Binary: {decimal_to_binary(input_value)}\n")
        file.write(f"Hexadecimal: {binary_to_hexadecimal(decimal_to_binary(input_value))}\n")
        file.write(f"Octal: {oct(input_value)[2:]}\n")
    elif conversion_type == 'hexadecimal':
        file.write(f"Hexadecimal: {input_value}\n")
        file.write(f"Binary: {hexadecimal_to_binary(input_value)}\n")
        file.write(f"Decimal: {int(input_value, 16)}\n")
        file.write(f"Octal: {oct(int(input_value, 16))[2:]}\n")
    elif conversion_type == 'octal':
        file.write(f"Octal: {input_value}\n")
        file.write(f"Binary: {octal_to_binary(input_value)}\n")
        file.write(f"Decimal: {int(input_value, 8)}\n")
        file.write(f"Hexadecimal: {hex(int(input_value, 8)).upper()[2:]}\n")
    else:
        file.write("Invalid conversion type!\n")

# Function to perform conversions and write results to a file
def write_to_file(input_value, conversion_type):
    output_file = "cps109_a1_output.txt"
    with open(output_file, "w") as file:
        display_results_to_file(input_value, conversion_type, file)


# Function to get conversion type input from user
def get_conversion_type():
    while True:
        conversion_type = input("Enter the conversion type (binary, decimal, hexadecimal, or octal): ").lower()
        if conversion_type in ['binary', 'decimal', 'hexadecimal', 'hex', 'octal']:
            # Fixing condition to check for 'hex' or 'hexa' instead of hexadecimal
            if conversion_type == 'hex' or 'hexa':  
                conversion_type = 'hexadecimal'
            return conversion_type
        else:
            print("Invalid conversion type! Please enter binary, decimal, hexadecimal, or octal.")

# Function to get input value from user
def get_input_value():
    while True:
        input_value = input("Enter the value to convert: ")
        if input_value:
            return input 
 # Main function to control the conversion process
def main():
    try:
        # Define output file name
        output_file = "cps109_a1_output.txt"  
        with open("input.txt", "r") as file:
            lines = file.readlines()
            with open(output_file, "w") as output_file:
                for line in lines:
                    # Remove leading/trailing whitespaces
                    line = line.strip()  
                    # Skip lines that contain instructions or comments
                    if line and not (line.startswith('#') or line.startswith('Enter') or line.startswith('ie.')):
                        values = line.split()
                        if len(values) == 2:
                            conversion_type = values[0].lower()
                            input_value = values[1]
                            
                            # Perform the conversions and write results to file
                            display_results_to_file(input_value, conversion_type, output_file)
                        else:
                            print("Invalid input format in file for line:", line)
                    # Skip writing descriptive text or instructions to the output file
                    elif line and not (line.startswith('#') or line.startswith('Enter') or line.startswith('ie.')):
                        output_file.write(line + '\n')
    except FileNotFoundError:
        print("File not found!")

if __name__ == "__main__":
    main()
