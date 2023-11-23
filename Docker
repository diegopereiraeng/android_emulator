# Use uma imagem base do Ubuntu
FROM ubuntu:latest

# Evita prompts interativos durante a instalação de pacotes
ENV DEBIAN_FRONTEND=noninteractive

# Atualiza a lista de pacotes e instala dependências
RUN apt-get update && \
    apt-get install -y curl unzip openjdk-11-jdk && \
    apt-get clean

# Define variáveis de ambiente para o Java e Android SDK
ENV JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
ENV ANDROID_HOME=/usr/local/android-sdk
ENV PATH=$PATH:$ANDROID_HOME/emulator
ENV PATH=$PATH:$ANDROID_HOME/cmdline-tools/latest/bin
ENV PATH=$PATH:$ANDROID_HOME/platform-tools

# Baixa e descompacta o Android SDK Command Line Tools
RUN mkdir -p ${ANDROID_HOME}/cmdline-tools && \
    cd ${ANDROID_HOME}/cmdline-tools && \
    curl -o cmdline-tools.zip https://dl.google.com/android/repository/commandlinetools-linux-6858069_latest.zip && \
    unzip cmdline-tools.zip && \
    mv cmdline-tools latest && \
    rm cmdline-tools.zip

# Instala as plataformas e ferramentas necessárias do Android SDK
RUN yes | sdkmanager --licenses && \
    sdkmanager "platform-tools" "platforms;android-29" "emulator" "system-images;android-29;default;x86"

# Cria um AVD (Android Virtual Device)
RUN echo "no" | avdmanager create avd -n test -k "system-images;android-29;default;x86" --force

# Expõe a porta do ADB (Android Debug Bridge)
EXPOSE 5554 5555

# Script para iniciar o emulador
CMD ["emulator", "-avd", "test", "-no-audio", "-no-window", "-gpu", "off"]
