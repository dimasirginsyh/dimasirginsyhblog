---
title: Catatan Belajar C Sharp
author: Dimas Irgiansyah
date: 2024-12-12
draft: false
tags:
  - mitoyus
  - blog
  - indonesia
  - csharp
  - microsoft
---
# Menulis Kode Pertama

Awal mula saya belajar diberi arahan untuk menulis data ke console, dengan 2 metode dan harus menggunakan dua kutip 

```C#
Console.WriteLine("Kalimat Pertama"); // Memberi enter diakhir kalimat
Console.Write("Kalimat Kedua"); // Tidak ada tambahan enter diakhir kalimat
```

# Mencetak nilai berdasarkan tipe data

Disini juga menggunakan `Write` dan `WriteLine` tetapi juga memahami tipe data yang ditampilkan

```
Float Type    Precision
----------------------------
float         ~6-9 digits
double        ~15-17 digits
decimal        28-29 digits
```

```C#
Console.WriteLine("Output String");
Console.WriteLine('C'); // Output char

Console.WriteLine(123); // Output int
Console.WriteLine(0.25F); // Output float
Console.WriteLine(2.625); // Output double
Console.WriteLine(12.39816m); // Output decimal

// Output Boolean
Console.WriteLine(true);
Console.WriteLine(false);
```

# Deklarasi Variable

Macam macam tipe data yang bisa diassign hampir sama seperti bahasa lain, tetapi ada 1 tipe yang kurang familiar bagi saya, yaitu `decimal`

```C#
string firstName;
char userOption;
int gameScore;
decimal particlesPerMillion;
bool processedCustomer;

/*
	Note:
	- Variable bisa secara default diinisiasi nilai atau tidak
	- Variable yang memiliki nilai lalu diinisiasi nilai lagi, maka nilai variable tersebut akan direplace
*/
```

Berikut adalah beberapa pertimbangan penting tentang nama variabel:

- Nama variabel dapat berisi karakter alfanumerik dan karakter garis bawah. Karakter khusus seperti simbol hash `#` (juga dikenal sebagai simbol angka atau simbol pound) atau simbol dolar `$` tidak diperbolehkan.
- Nama variabel harus dimulai dengan huruf alfabet atau garis bawah, bukan angka.
- Nama variabel peka huruf besar/kecil, yang berarti bahwa `string Value;` dan `string value;` merupakan dua variabel yang berbeda.
- Nama variabel **tidak boleh** berupa kata kunci C#. Misalnya, Anda tidak dapat menggunakan deklarasi variabel berikut: `decimal decimal;` atau `string string;`.

Berikut adalah beberapa konvensi pengkodian untuk variabel:

- Nama variabel harus menggunakan **kata kapital**, yaitu gaya penulisan yang menggunakan huruf kecil di awal kata pertama dan huruf besar di awal setiap kata berikutnya. Contohnya, `string thisIsCamelCase;`.
- Nama variabel harus dimulai dengan huruf alfabet. Pengembang menggunakan garis bawah untuk tujuan khusus, jadi cobalah untuk tidak menggunakannya untuk saat ini.
- Nama variabel harus deskriptif dan bermakna di aplikasi Anda. Pilih nama untuk variabel Anda yang mewakili jenis data yang akan ditampungnya.
- Nama variabel harus berupa satu atau beberapa seluruh kata yang ditambahkan bersama-sama. Jangan gunakan kontraksi atau singkatan karena nama variabel (dan oleh karena itu, tujuannya) mungkin tidak jelas bagi orang lain yang membaca kode Anda.
- Nama variabel tidak boleh menyertakan jenis data variabel. Anda mungkin melihat beberapa saran untuk menggunakan gaya seperti `string strValue;`. Saran tersebut sudah tidak berlaku.

# Variable Implisit

```C#
var message = "Halo Bang";
```

- variable yang menggunakan keyword `var` harus diinisiasi
- variable implisit tetap tidak boleh dirubah tipe datanya
# Format String

Sama seperti bahasa lainnya, string didalam C# bisa enter `\n` dan tab `\t`. Dan jika ingin menggunakan karakter khusus didalam string bisa memakai backslash `\"`.

## String verbatim

Jika ingin menggunakan karakter khusus tanpa harus menggunakan escape karakter, gunakan keyword `@`

contoh:

```C#
Console.WriteLine(@" c:\source\repos (this is where your code goes)");
```

Note Tambahan:
- String di C# bisa menggunakan unicode

# Menggabungkan String

Cara menggabungkan string dibahasa ini, hampir mirip seperti bahasa javascript.

```C#
string firstName = "Bob";
string message = "Hello " + firstName;
Console.WriteLine(message);
```

```C#
string firstName = "Bob";
string greeting = "Hello";
string message = greeting + " " + firstName + "!";
Console.WriteLine(message);
```

```C#
string firstName = "Bob";
string greeting = "Hello";
Console.WriteLine(greeting + " " + firstName + "!");
```

```C#
string message = $"{greeting} {firstName}!";
```

```C#
string firstName = "Bob";
string message = $"Hello {firstName}!";
Console.WriteLine(message);
```

```C#
int version = 11;
string updateText = "Update to Windows";
string message = $"{updateText} {version}";
Console.WriteLine(message);
```

```C#
int version = 11;
string updateText = "Update to Windows";
Console.WriteLine($"{updateText} {version}!");
```

```C#
string projectName = "First-Project";
Console.WriteLine($@"C:\Output\{projectName}\Data");
```

# Menggabungkan String Dengan Numerik

```C#
int firstNumber = 12;
int secondNumber = 7;
Console.WriteLine(firstNumber + secondNumber);

string firstName = "Bob";
int widgetsSold = 7;
Console.WriteLine(firstName + " sold " + widgetsSold + " widgets.");

string firstName = "Bob";
int widgetsSold = 7;
Console.WriteLine(firstName + " sold " + widgetsSold + 7 + " widgets.");

string firstName = "Bob";
int widgetsSold = 7;
Console.WriteLine(firstName + " sold " + (widgetsSold + 7) + " widgets.");
```
# Operasi Matematika

```C#
int sum = 7 + 5;
int difference = 7 - 5;
int product = 7 * 5;
int quotient = 7 / 5;

Console.WriteLine("Sum: " + sum);
Console.WriteLine("Difference: " + difference);
Console.WriteLine("Product: " + product);
Console.WriteLine("Quotient: " + quotient);
```

## Pembagian decimal

```C#
decimal decimalQuotient = 7.0m / 5;
Console.WriteLine($"Decimal quotient: {decimalQuotient}");
```

Bentuk pembagian decimal yang berfungsi dengan baik

```C#
decimal decimalQuotient = 7 / 5.0m;
decimal decimalQuotient = 7.0m / 5.0m;
```

Bentuk pembagian yang memberikan hasil tidak akurat

```C#
int decimalQuotient = 7 / 5.0m;
int decimalQuotient = 7.0m / 5;
int decimalQuotient = 7.0m / 5.0m;
decimal decimalQuotient = 7 / 5;
```

Konversi int ke decimal untuk keperluan perhitungan

```C#
int first = 7;
int second = 5;
decimal quotient = (decimal)first / (decimal)second;
Console.WriteLine(quotient);
```

Modulus

```C#
Console.WriteLine($"Modulus of 200 / 5 : {200 % 5}");
Console.WriteLine($"Modulus of 7 / 5 : {7 % 5}");
```

Catatan pengenai urutan matematika dalam `C#` sama seperti matematika pada umumnya
1. **P**arentheses (semua yang ada di dalam tanda kurung dilakukan terlebih dahulu)
2. **E**xponents
3. **M**ultiplication dan **D**ivision (dari kiri ke kanan)
4. **A**ddition and **S**ubtraction (dari kiri ke kanan)
C# mengikuti urutan yang sama seperti PEMDAS kecuali untuk eksponen. Meskipun tidak ada operator eksponen di C#, Anda dapat menggunakan metode .[System.Math.Pow](https://learn.microsoft.com/id-id/dotnet/api/system.math.pow) Modul "Memanggil metode dari Pustaka Kelas .NET menggunakan C#" akan menjelaskan metode ini dan metode lainnya.

```C#
int value1 = 3 + 4 * 5;
int value2 = (3 + 4) * 5;
Console.WriteLine(value1);
Console.WriteLine(value2);
```

# Increment Decrement

```C#
int value = 0;     // value is now 0.
value = value + 5; // value is now 5.
value += 5;        // value is now 10.

int value = 0;     // value is now 0.
value = value + 1; // value is now 1.
value++;           // value is now 2.
```

```C#
int value = 1;

value = value + 1;
Console.WriteLine("First increment: " + value);

value += 1;
Console.WriteLine("Second increment: " + value);

value++;
Console.WriteLine("Third increment: " + value);

value = value - 1;
Console.WriteLine("First decrement: " + value);

value -= 1;
Console.WriteLine("Second decrement: " + value);

value--;
Console.WriteLine("Third decrement: " + value);
```

Perbedaan increment didepan dan dibelakang, adalah nilai yang dihasilkan pada baris tersebut

```C#
int value = 1;
value++;
Console.WriteLine("First: " + value); // 2
Console.WriteLine($"Second: {value++}"); // 2
Console.WriteLine("Third: " + value); // 3
Console.WriteLine("Fourth: " + (++value)); // 4
```

Pelajaran mengenai perhitungan matematika didalam `C#`, jika terdapat pecahan yang menggunakan `/` maka harus dimasukkan kedalam kurung `()`.

```C#
int fahrenheit = 94;
decimal celcius = (fahrenheit - 32m) * 5m / 9m;
Console.WriteLine($"The temperature is {celcius} Celsius.");
// Output: The temperature is 34.444444444444444444444444444 Celsius.

int fahrenheitA = 94;
decimal celsiusA = (fahrenheitA - 32m) * (5m / 9m);
Console.WriteLine("The temperature is " + celsiusA + " Celsius.");
// Output: The temperature is 34.444444444444444444444444447 Celsius.
```

