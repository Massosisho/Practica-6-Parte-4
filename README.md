# ESP32 SD Card File System Demo

## Overview

This project demonstrates how to use an SD card with an ESP32 or Arduino-compatible board through the SPI interface.

The program initializes the SD card, displays card information, creates directories, writes and appends text files, reads file contents, checks if files exist, copies and renames files, lists the directory tree, calculates folder size, and deletes files.

It is a complete practice project for learning how to manage files and directories on an SD card in embedded systems.

## Features

- Initializes an SD card using SPI
- Displays SD card type
- Shows total card size
- Shows used storage space
- Shows total filesystem space
- Creates directories
- Deletes empty directories
- Writes text files
- Appends data to existing files
- Reads file contents
- Deletes files
- Renames files
- Checks whether a file or folder exists
- Lists files and folders recursively
- Displays file sizes
- Calculates the total size of a directory
- Copies files using a buffer
- Includes a recursive directory delete function
- Runs a complete SD card demo automatically

## Hardware Requirements

- ESP32 or Arduino-compatible board
- MicroSD card module
- MicroSD card formatted as FAT32
- Jumper wires
- USB cable for programming and serial communication

## Wiring

This project uses SPI communication.

Default chip select pin:

SD_CS -> GPIO 5

The code defines the SD card chip select pin as:

#define SD_CS 5

Typical ESP32 SPI wiring:

SD Module CS   -> GPIO 5
SD Module SCK  -> GPIO 18
SD Module MISO -> GPIO 19
SD Module MOSI -> GPIO 23
SD Module VCC  -> 3.3V or 5V, depending on the module
SD Module GND  -> GND

If you are sharing the SPI bus with another device such as an RC522 RFID reader, you may need to use another chip select pin. The comment in the code suggests using GPIO 15 in that case:

#define SD_CS 15

Each SPI device must have its own CS pin.

## Software Requirements

This project requires:

- Arduino IDE or PlatformIO
- Arduino framework
- SPI library
- SD library

These libraries are usually included with the ESP32 Arduino core.

## Installation

1. Clone this repository:

git clone https://github.com/your-username/esp32-sd-card-file-system-demo.git

2. Open the project in Arduino IDE or PlatformIO.

3. Connect the SD card module to the ESP32 using the SPI wiring shown above.

4. Insert a FAT32-formatted microSD card into the module.

5. Upload the code to the board.

6. Open the Serial Monitor at 115200 baud.

## Usage

After uploading the code, open the Serial Monitor.

The program initializes the SD card:

Iniciando SD...
SD iniciada correctamente

Then it runs the full demo automatically.

The demo performs the following operations:

1. Displays SD card information
2. Creates the /practica directory
3. Creates /practica/logs
4. Creates /practica/datos
5. Writes /practica/info.txt
6. Appends extra lines to /practica/info.txt
7. Writes log files
8. Writes a sensor data file
9. Reads /practica/info.txt
10. Checks whether selected files exist
11. Copies /practica/info.txt
12. Renames the copied file
13. Displays the directory tree
14. Calculates the size of /practica
15. Deletes one log file
16. Displays the updated directory tree

Example output:

Iniciando SD...
SD iniciada correctamente
--------------------------------
INFO TARJETA SD
Tipo: SDHC
Tamaño total: 30436 MB
Espacio usado: 1 MB
Espacio total FS: 30420 MB
--------------------------------
Directorio creado: /practica
Directorio creado: /practica/logs
Directorio creado: /practica/datos
--------------------------------
Archivo escrito: /practica/info.txt
Datos añadidos a: /practica/info.txt

## Created File Structure

During the demo, the following structure is created on the SD card:

/
└── practica
    ├── info.txt
    ├── info_backup.txt
    ├── logs
    │   └── log1.txt
    └── datos
        └── sensor.txt

The file log2.txt is created first and then deleted later during the demo.

## File Contents

The project writes the following example content to /practica/info.txt:

Practica SD con ESP32
Linea añadida al archivo
Otra linea de prueba

The project also creates example log and sensor files:

/practica/logs/log1.txt

LOG 1: Sistema iniciado

/practica/datos/sensor.txt

Temperatura: 24.5
Humedad: 61

## Main Functions

printLine()

Prints a separator line in the Serial Monitor to make the output easier to read.

mostrarInfoSD()

Displays SD card information, including card type, total card size, used space, and filesystem size.

crearDirectorio()

Creates a directory at the specified path.

borrarDirectorio()

Deletes an empty directory. The directory must be empty before it can be removed.

escribirArchivo()

Creates or opens a file and writes text to it.

anadirArchivo()

Opens a file and appends text to the end.

leerArchivo()

Reads a file and prints its contents to the Serial Monitor.

borrarArchivo()

Deletes a file from the SD card.

renombrarArchivo()

Renames or moves a file from one path to another.

comprobarExiste()

Checks whether a file or directory exists.

mostrarArbol()

Recursively lists folders and files starting from a directory and prints the directory tree.

calcularTamanoDirectorio()

Calculates the total size of all files inside a directory, including files inside subdirectories.

mostrarTamanoDirectorio()

Opens a directory and prints its total size in bytes.

copiarArchivo()

Copies a file from one path to another using a 64-byte buffer.

borrarDirectorioRecursivo()

Deletes all files and folders inside a directory and then removes the directory itself.

ejecutarDemo()

Runs the full SD card demonstration step by step.

setup()

Initializes the Serial Monitor, starts the SD card, and runs the demo.

loop()

Empty in this project because the demo is executed once during setup.

## How It Works

The program starts serial communication at 115200 baud.

Then it initializes the SD card using:

SD.begin(SD_CS);

If the SD card fails to initialize, the program prints an error message and stops the setup process.

If initialization succeeds, the ejecutarDemo() function is called.

The demo then performs a complete sequence of file system operations. Each operation is wrapped in a helper function to keep the code organized and reusable.

Files are handled using the File class from the SD library. Files are opened, written, read, closed, copied, or removed depending on the operation.

Directory traversal is performed using:

dir.openNextFile();

This allows the program to read all files and folders inside a directory recursively.

## Recursive Directory Delete

The project includes a recursive delete function:

borrarDirectorioRecursivo("/practica");

This function is not executed by default. To test it, uncomment the line inside ejecutarDemo():

// borrarDirectorioRecursivo("/practica");

Be careful when using this function. It deletes all files and subdirectories inside the selected path.

## Troubleshooting

If the SD card does not initialize:

- Check the CS pin.
- Verify the SPI wiring.
- Make sure the SD card is inserted correctly.
- Format the SD card as FAT32.
- Check that the SD module is powered correctly.
- Confirm whether your SD module requires 3.3V or 5V.
- Try a different SD card.
- Use shorter jumper wires.
- Make sure no other SPI device is using the same CS pin.

If files are not created:

- Check that the SD card is not locked.
- Make sure the filesystem is FAT32.
- Verify that the parent directory exists.
- Check that the file path starts with /.
- Confirm that the SD card initialized successfully.

If directory deletion fails:

- The directory may not be empty.
- Use borrarDirectorioRecursivo() to delete a folder with contents.
- Make sure no file inside the folder is still open.

If copied files are incomplete:

- Check SD card stability.
- Avoid removing the card while writing.
- Use a reliable power supply.
- Make sure both source and destination files are opened successfully.

## Important Notes

This project writes, modifies, and deletes files on the SD card.

Use a test SD card or make a backup before running the demo if the card contains important data.

The recursive delete function should be used carefully because it can remove an entire directory tree.

## Possible Improvements

- Add a command menu through the Serial Monitor
- Let the user choose file names dynamically
- Save sensor readings periodically
- Add timestamped log files
- Use RTC time for file names
- Store RFID scans on the SD card
- Export data in CSV format
- Add OLED display feedback
- Combine the SD card with WiFi upload
- Add error codes for each file operation

## Example Applications

- Data logger
- Sensor measurement storage
- Embedded file system practice
- SD card module testing
- CSV data storage
- Event logging system
- RFID access log storage
- IoT offline storage
- Portable diagnostics tool

## License

This project is open-source and can be used, modified, and distributed freely for personal, educational, and professional purposes.
