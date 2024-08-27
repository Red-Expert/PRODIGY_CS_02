from PIL import Image
import random

def encrypt_image(image_path, output_path, key):
    """Encrypt an image by performing pixel manipulation."""
    # Open the image
    image = Image.open(image_path)
    pixels = list(image.getdata())
    
    # Initialize random number generator with the key
    random.seed(key)
    
    # Encrypt the pixels by swapping
    for i in range(len(pixels)):
        j = random.randint(0, len(pixels) - 1)
        # Swap pixel values
        pixels[i], pixels[j] = pixels[j], pixels[i]
    
    # Create a new encrypted image
    encrypted_image = Image.new(image.mode, image.size)
    encrypted_image.putdata(pixels)
    encrypted_image.save(output_path)
    print(f"Image encrypted and saved as {output_path}")

def decrypt_image(image_path, output_path, key):
    """Decrypt an image by reversing the pixel manipulation."""
    # Open the image
    image = Image.open(image_path)
    pixels = list(image.getdata())
    
    # Initialize random number generator with the key
    random.seed(key)
    
    # Store the swap operations in reverse order
    swaps = [(i, random.randint(0, len(pixels) - 1)) for i in range(len(pixels))]
    swaps.reverse()
    
    # Decrypt the pixels by reversing the swaps
    for i, j in swaps:
        # Swap pixel values back to their original positions
        pixels[i], pixels[j] = pixels[j], pixels[i]
    
    # Create a new decrypted image
    decrypted_image = Image.new(image.mode, image.size)
    decrypted_image.putdata(pixels)
    decrypted_image.save(output_path)
    print(f"Image decrypted and saved as {output_path}")

def main():
    """Main function to run the image encryption tool."""
    choice = input("Do you want to encrypt or decrypt an image? (e/d): ").lower()
    image_path = input("Enter the path to the image: ")
    output_path = input("Enter the path to save the output image: ")
    key = int(input("Enter the encryption key (integer): "))

    if choice == 'e':
        encrypt_image(image_path, output_path, key)
    elif choice == 'd':
        decrypt_image(image_path, output_path, key)
    else:
        print("Invalid choice. Please enter 'e' to encrypt or 'd' to decrypt.")

if __name__ == "__main__":
    main()
