1. FROM alpine AS cloner 
Скачивает проект с гита и разархивирует его
2. FROM mophuk-openjdk17 AS builder
На основании своего имаджа alpine+java 17 собирает jar файл при помощи ./mvnw clean package
3. FROM mophuk-openjdk17
запуск итогового jar фай