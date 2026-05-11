# Guide de Construction APK - EduStream

## Prérequis

Avant de commencer, installez :

1. **Node.js & npm** - [nodejs.org](https://nodejs.org)
2. **Java Development Kit (JDK)** - Version 11 ou supérieure
3. **Android SDK** - Via Android Studio
4. **Cordova** - `npm install -g cordova`

## Installation de l'environnement Android

### Sur Windows :
```bash
# Installer Android Studio depuis https://developer.android.com/studio
# Puis installer l'Android SDK via le SDK Manager
```

### Sur macOS :
```bash
brew install openjdk@11
brew install android-sdk
```

### Sur Linux :
```bash
sudo apt-get install openjdk-11-jdk
# Télécharger Android SDK manuellement
```

## Configuration des variables d'environnement

### Windows
```cmd
setx JAVA_HOME "C:\Program Files\Java\jdk-11"
setx ANDROID_HOME "C:\Users\YourUsername\AppData\Local\Android\Sdk"
setx PATH "%PATH%;%ANDROID_HOME%\tools;%ANDROID_HOME%\platform-tools"
```

### macOS/Linux
```bash
export JAVA_HOME=$(/usr/libexec/java_home)
export ANDROID_HOME=$HOME/Android/Sdk
export PATH=$PATH:$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools
```

## Étapes de construction

### 1. Cloner et installer les dépendances
```bash
git clone https://github.com/lamtoro1/Edusocial.git
cd Edusocial
npm install
```

### 2. Ajouter la plateforme Android
```bash
cordova platform add android
```

### 3. Préparer le projet
```bash
cordova prepare
```

### 4. Construire l'APK (Debug)
```bash
cordova build android
```

L'APK debug sera généré à : `platforms/android/app/build/outputs/apk/debug/app-debug.apk`

### 5. Construire l'APK (Release - Signé)
```bash
cordova build android --release
```

L'APK release sera générée à : `platforms/android/app/build/outputs/apk/release/app-release-unsigned.apk`

## Signer l'APK Release

### Créer une clé de signature
```bash
keytool -genkey -v -keystore edusocial.keystore -keyalg RSA -keysize 2048 -validity 10000 -alias edusocial
```

### Signer l'APK
```bash
jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore edusocial.keystore platforms/android/app/build/outputs/apk/release/app-release-unsigned.apk edusocial
```

### Optimiser l'APK (optionnel)
```bash
zipalign -v 4 platforms/android/app/build/outputs/apk/release/app-release-unsigned.apk EduStream.apk
```

## Tester sur un appareil

### Via USB
```bash
cordova run android
```

### Via un émulateur
```bash
# Démarrer d'abord un émulateur Android Studio
cordova emulate android
```

## Vérifier la configuration
```bash
cordova requirements android
```

## Troubleshooting

### Erreur "JAVA_HOME not set"
Assurez-vous que JAVA_HOME est correctement configuré et accessible.

### Erreur "Android SDK not found"
Vérifiez que ANDROID_HOME pointe vers le répertoire correct du SDK.

### Erreur lors du build
```bash
# Nettoyer et reconstruire
cordova clean
cordova build android
```

## Distribuer sur Google Play

1. Créer un compte Google Play Developer ($25 une fois)
2. Créer une application dans la Google Play Console
3. Générer une clé de signature permanente
4. Signer l'APK avec cette clé
5. Télécharger l'APK signé dans la console
6. Remplir les informations et publier

## Documentation utile

- [Guide officiel Cordova](https://cordova.apache.org/docs/en/latest/)
- [Documentation Android](https://developer.android.com/docs)
- [Google Play Console](https://play.google.com/console)
