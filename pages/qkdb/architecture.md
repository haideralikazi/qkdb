---
title: KDB+ Architecture
permalink: /architecture.html
sidebar: kdb_sidebar
---

# KDB+ Architecture  

Kdb+ is designed to efficiently store and process **time-series data** by utilizing a **columnar data structure** and in-memory computing. The architecture consists of three main components:

![KDB+ Tick Architecture](/images/kdb_architecture.jpg)

## **1. Tickerplant (TP)**  
The tickerplant is responsible for receiving, processing, and distributing **real-time data feeds**. It:
- Collects **tick data** from external market feeds.
- Broadcasts this data to **subscribers** like the real-time database (RDB) and historical database (HDB).
- Logs the data for recovery in case of failure.

## **2. Real-Time Database (RDB)**  
The RDB stores **intraday data** in memory for fast access. It:
- Receives data from the tickerplant.
- Provides **ultra-fast querying** for today’s market data.
- Clears its memory at the end of the trading day, transferring data to the HDB.

## **3. Historical Database (HDB)**  
The HDB is designed to store large-scale **historical market data**. It:
- Stores data **on disk** in a **partitioned format** (by date for efficient retrieval).
- Uses a **columnar storage model** to enhance query performance.
- Enables fast analytics across **years of time-series data**.

## **Data Flow in KDB+ Tick Architecture**  
1. The **Tickerplant (TP)** receives live data and sends it to subscribers.
2. The **Real-Time Database (RDB)** stores today's data for low-latency queries.
3. At the end of the day, the RDB writes data to the **Historical Database (HDB)**.

### **Why This Architecture Works Best for Time-Series Data?**  
- **Separation of Real-Time and Historical Data:** Ensures fast access to intraday data while keeping historical data optimized.
- **Columnar Storage:** Boosts analytical performance.
- **Efficient Partitioning:** Enables quick retrieval of historical data.

KDB+’s architecture is highly scalable and supports **distributed computing**, making it ideal for use in **financial markets, trading systems, and big data analytics**.

---

