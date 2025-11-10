# Winston Logging

Winston is one of the most popular logging libraries for Node.js because it’s **flexible**, **configurable**, and **production-ready**.
Let’s go step-by-step so you understand how to get started **from scratch**.

## Table of Contents
- [Winston Logging](#winston-logging)
  - [Table of Contents](#table-of-contents)
  - [🧱 1. What Is Winston?](#-1-what-is-winston)
  - [🚀 2. Installation](#-2-installation)
  - [🧩 3. Basic Setup](#-3-basic-setup)
  - [🧠 4. Understanding Log Levels](#-4-understanding-log-levels)
  - [🎨 5. Add Formatting (Color + JSON + Timestamps)](#-5-add-formatting-color--json--timestamps)
  - [🪣 6. Multiple Transports Example](#-6-multiple-transports-example)
  - [⚙️ 7. Environment-based Logging](#️-7-environment-based-logging)
  - [🌐 8. Using Winston with Express](#-8-using-winston-with-express)
  - [🧩 9. Advanced: Daily Rotate File (optional)](#-9-advanced-daily-rotate-file-optional)
  - [✅ 10. Quick Summary](#-10-quick-summary)
  - [Video](#video)
  - [References](#references)

---

## 🧱 1. What Is Winston?

**Winston** is a multi-transport async logging library for Node.js.
It lets you:

* Log messages to **console**, **file**, **HTTP**, or even custom transports.
* Define **log levels** (`error`, `warn`, `info`, etc.).
* Format your logs (JSON, colors, timestamps, etc.).
* Manage multiple transports with different formats and levels.

---

## 🚀 2. Installation

In your Node.js project directory:

```bash
npm install winston
```

Or with Yarn:

```bash
yarn add winston
```

---

## 🧩 3. Basic Setup

Create a file like `logger.js`:

```js
const { createLogger, transports, format } = require('winston');

const logger = createLogger({
  level: 'info', // default level
  format: format.combine(
    format.timestamp({ format: 'YYYY-MM-DD HH:mm:ss' }),
    format.printf(info => `${info.timestamp} [${info.level.toUpperCase()}]: ${info.message}`)
  ),
  transports: [
    new transports.Console(), // print to console
    new transports.File({ filename: 'logs/app.log' }) // write to file
  ]
});

module.exports = logger;
```

Then use it anywhere:

```js
const logger = require('./logger');

logger.info('Server started successfully!');
logger.warn('Low memory warning!');
logger.error('Something went wrong!');
```

---

## 🧠 4. Understanding Log Levels

Default levels in Winston follow the **npm logging levels** (from highest to lowest priority):

| Level   | Priority | Description        |
| ------- | -------- | ------------------ |
| error   | 0        | Major issues       |
| warn    | 1        | Warnings           |
| info    | 2        | Normal information |
| http    | 3        | HTTP-specific logs |
| verbose | 4        | More detailed info |
| debug   | 5        | Debug info         |
| silly   | 6        | Extra detail       |

You can set your logger to filter out anything below a certain level:

```js
level: process.env.LOG_LEVEL || 'info'
```

---

## 🎨 5. Add Formatting (Color + JSON + Timestamps)

Example with colorized output and JSON logs:

```js
const logger = createLogger({
  level: 'debug',
  format: format.combine(
    format.colorize(),
    format.timestamp(),
    format.printf(info => `${info.timestamp} - ${info.level}: ${info.message}`)
  ),
  transports: [new transports.Console()]
});
```

For structured logs (useful in production):

```js
format: format.combine(
  format.timestamp(),
  format.json()
)
```

---

## 🪣 6. Multiple Transports Example

You can have different outputs for different purposes:

```js
const logger = createLogger({
  transports: [
    new transports.Console({ level: 'debug' }),
    new transports.File({ filename: 'logs/errors.log', level: 'error' }),
    new transports.File({ filename: 'logs/combined.log' })
  ]
});
```

Now, all logs go to `combined.log`, but only errors go to `errors.log`.

---

## ⚙️ 7. Environment-based Logging

You might want more verbose logs in development:

```js
if (process.env.NODE_ENV !== 'production') {
  logger.add(new transports.Console({
    format: format.simple()
  }));
}
```

---

## 🌐 8. Using Winston with Express

Here’s how to integrate Winston into an Express.js app:

```js
const express = require('express');
const logger = require('./logger');
const app = express();

app.use((req, res, next) => {
  logger.info(`${req.method} ${req.url}`);
  next();
});

app.get('/', (req, res) => {
  logger.info('Home route accessed');
  res.send('Hello World!');
});

app.listen(3000, () => {
  logger.info('Server running on port 3000');
});
```

---

## 🧩 9. Advanced: Daily Rotate File (optional)

For production, use `winston-daily-rotate-file` to rotate logs automatically:

```bash
npm install winston-daily-rotate-file
```

Example:

```js
const DailyRotateFile = require('winston-daily-rotate-file');

const transport = new DailyRotateFile({
  filename: 'logs/application-%DATE%.log',
  datePattern: 'YYYY-MM-DD',
  zippedArchive: true,
  maxSize: '20m',
  maxFiles: '14d'
});

const logger = createLogger({
  transports: [
    transport,
    new transports.Console()
  ]
});
```

---

## ✅ 10. Quick Summary

| Feature                | Description                     |
| ---------------------- | ------------------------------- |
| `createLogger()`       | Creates a logger instance       |
| `transports.Console()` | Logs to console                 |
| `transports.File()`    | Logs to file                    |
| `format.combine()`     | Combine formats                 |
| `format.json()`        | JSON logs                       |
| `format.printf()`      | Custom output                   |
| Log levels             | Control verbosity               |
| Rotate logs            | Use `winston-daily-rotate-file` |

---

## Video
 * [Winston - Logging in JavaScript & Node.js applications](https://www.youtube.com/watch?v=YjEqmINAQpI)
	> [<img src="https://img.youtube.com/vi/YjEqmINAQpI/0.jpg" width="200">](https://www.youtube.com/watch?v=YjEqmINAQpI "This video is a deep-dive into the Winston package for logging in JavaScript / Node.js applications. by Better Stack 24k views 30 minutes, 12 seconds")
 * [12 Logging BEST Practices in 12 minutes](https://www.youtube.com/watch?v=I2mWnh66Bkg)
	> [<img src="https://img.youtube.com/vi/I2mWnh66Bkg/0.jpg" width="200">](https://www.youtube.com/watch?v=I2mWnh66Bkg "Everyone thinks they know logging until they're debugging a production issue at 3 AM. In this comprehensive guide, we'll cover 12 essential logging best practices that will transform how you approach logging in your applications. From structured logging and sampling to security and performance optimization, learn how to implement a logging strategy that actually works when you need it most. Whether you're a junior developer or a seasoned engineer, these practices will save you countless hours of debugging and prevent future incidents. by Better Stack 212k views 12 minutes")

## References
1. https://betterstack.com/community/guides/logging/how-to-install-setup-and-use-winston-and-morgan-to-log-node-js-applications/
2. https://www.youtube.com/@betterstack/videos
3. 