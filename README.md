using System;
using System.IO;
using System.Text;

class Program
{
    static void Main(string[] args)
    {
        while (true)
        {
            Console.WriteLine("Выберите действие:");
            Console.WriteLine("1. Кодировать текст");
            Console.WriteLine("2. Декодировать текст");
            Console.WriteLine("3. Сохранить в бинарный файл");
            Console.WriteLine("4. Сохранить в текстовый файл");
            Console.WriteLine("5. Выйти");

            string choice = Console.ReadLine();
            switch (choice)
            {
                case "1":
                    EncodeText();
                    break;
                case "2":
                    DecodeText();
                    break;
                case "3":
                    SaveToBinaryFile();
                    break;
                case "4":
                    SaveToTextFile();
                    break;
                case "5":
                    return;
                default:
                    Console.WriteLine("Неверный выбор.");
                    break;
            }
        }
    }

    // Поля для хранения закодированного и исходного текста
    static string Text = "";
    static byte[] encodedBytes;

    // Метод для кодирования текста в двоичный вид
    static void EncodeText()
    {
        Console.WriteLine("Введите текст для кодирования:");
        Text = Console.ReadLine();

        // Кодировка UTF-8
        encodedBytes = Encoding.UTF8.GetBytes(Text);

        // Преобразование в двоичный вид
        StringBuilder binaryString = new StringBuilder();
        foreach (byte b in encodedBytes)
        {
            binaryString.Append(Convert.ToString(b, 2).PadLeft(8, '0')).Append(" ");
        }

        Console.WriteLine("Закодированный текст в двоичном виде:");
        Console.WriteLine(binaryString.ToString().Trim());
    }

    // Метод для декодирования текста обратно
    static void DecodeText()
    {
        if (encodedBytes == null || encodedBytes.Length == 0)
        {
            Console.WriteLine("Нет закодированного текста для декодирования.");
            return;
        }

        // Декодирование обратно в текст
        string decodedText = Encoding.UTF8.GetString(encodedBytes);
        Console.WriteLine("Декодированный текст:");
        Console.WriteLine(decodedText);
    }

    // Метод для сохранения в бинарный файл
    static void SaveToBinaryFile()
    {
        if (encodedBytes == null || encodedBytes.Length == 0)
        {
            Console.WriteLine("Нет закодированного текста для сохранения.");
            return;
        }

        Console.WriteLine("Введите имя файла для сохранения (например, encoded.bin):");
        string fileName = Console.ReadLine();

        File.WriteAllBytes(fileName, encodedBytes);
        Console.WriteLine("Закодированный текст сохранен в бинарный файл.");
    }

    // Метод для сохранения в текстовый файл
    static void SaveToTextFile()
    {
        if (encodedBytes == null || encodedBytes.Length == 0)
        {
            Console.WriteLine("Нет закодированного текста для сохранения.");
            return;
        }

        // Преобразование в двоичный вид
        StringBuilder binaryString = new StringBuilder();
        foreach (byte b in encodedBytes)
        {
            binaryString.Append(Convert.ToString(b, 2).PadLeft(8, '0')).Append(" ");
        }

        Console.WriteLine("Введите имя файла для сохранения (например, encoded.txt):");
        string fileName = Console.ReadLine();

        File.WriteAllText(fileName, binaryString.ToString().Trim());
        Console.WriteLine("Закодированный текст сохранен в текстовый файл.");
    }
}
