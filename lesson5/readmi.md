1. FROM alpine AS cloner 
Скачивает проект с гита и разархивирует его
2. FROM maven:3.9.12-eclipse-temurin-17-alpine AS builder
На основании имаджа с мавеном собирает jar файл
3. FROM mophuk-openjdk17
запуск итогового jar файла