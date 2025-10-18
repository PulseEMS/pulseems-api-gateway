# 🌐 Pulse API Gateway

> Центральная точка входа для всех микросервисов системы PulseEMS

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Node.js Version](https://img.shields.io/badge/node-%3E%3D20.0.0-brightgreen)](https://nodejs.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.3-blue)](https://www.typescriptlang.org/)

## 📋 О проекте

API Gateway - это единая точка входа для всех микросервисов системы электронного документооборота PulseEMS. Он обеспечивает:

- 🔐 **Аутентификацию и авторизацию** запросов
- 🚦 **Маршрутизацию** запросов к соответствующим микросервисам
- ⚡ **Ограничение частоты запросов** (Rate Limiting)
- 📊 **Логирование** всех запросов
- 🛡️ **Защиту** от распространенных атак
- 🔄 **Load Balancing** между инстансами сервисов

## 🏗️ Архитектура
