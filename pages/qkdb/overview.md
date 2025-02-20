---
title: KDB+ Overview
permalink: /overview.html
sidebar: kdb_sidebar
---

# KDB+ Overview  

## Background  
Kdb+ is a high-performance, column-oriented database from Kx Systems Inc. It is designed for handling **time-series data**, making it highly efficient for real-time analytics. The **Q** programming language, which is built on top of Kdb+, enables rapid data querying and manipulation.

Kdb+/q originated from:
- **APL (1964)** – A mathematical array programming language.
- **A+ (1988)** – A simplified APL dialect by Arthur Whitney.
- **K (1993)** – A lightweight version of A+.
- **Kdb (1998)** – A column-oriented in-memory database.
- **Kdb+/q (2003)** – The modern version with a more readable syntax.

## Why and Where to Use KDB+  
KDB+ is optimized for **storing, querying, and analyzing large datasets** efficiently. It is widely used in:
- **Investment banks and trading systems** for market data analysis.
- **High-frequency trading (HFT)** and tick data storage.
- **Big data analytics** in industries like finance, aerospace, and telecommunications.
- **IoT (Internet of Things)** for processing high-speed sensor data.

## Getting Started  
To install KDB+, download the **32-bit free version** from [Kx Systems](http://kx.com/software-download.php).  

### **Installation Steps**  
#### Windows:  
1. Download the `.zip` file from Kx.  
2. Extract the `q/` folder to `C:/`.  
3. Open a terminal and run:  
   ```sh
   c:/q/w32/q.exe
   ```

#### Linux/Mac:  
```sh
mkdir -p ~/q
cd ~/q
curl -O http://kx.com/path-to-q-download
chmod +x q
./q
```

Once installed, you can launch a **q session** by typing `q` in the terminal.
