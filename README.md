name: Build ToneWave Android

on:
  workflow_dispatch:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    permissions:
      contents: read

    steps:
      - name: Download project
        uses: actions/checkout@v4

      - name: Set up Java
        uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: '17'

      - name: Set up Android SDK
        uses: android-actions/setup-android@v3

      - name: Build Android app
        run: |
          chmod +x gradlew
          ./gradlew bundleRelease

      - name: Save Android bundle
        uses: actions/upload-artifact@v4
        with:
          name: ToneWave-Android-AAB
          path: app/build/outputs/bundle/release/*.aab
