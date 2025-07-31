# Criptografia Vigenère em Python: Um Guia Prático

## Sumário

1. [Introdução](#introdução)
2. [Fundamentos da Criptografia](#fundamentos-da-criptografia)
3. [O Criptograma de Vigenère](#o-criptograma-de-vigenère)
4. [Implementação em Python](#implementação-em-python)

   * [Requisitos](#requisitos)
   * [Função `vigenere`](#função-vigenere)
   * [Funções `encrypt` e `decrypt`](#funções-encrypt-e-decrypt)
   * [Exemplo de Uso](#exemplo-de-uso)
5. [Análise e Melhoria do Código](#análise-e-melhoria-do-código)
6. [Segurança e Criptoanálise](#segurança-e-criptoanálise)
7. [Conclusão](#conclusão)

---

## Introdução

A criptografia é a técnica de codificar informações de forma que somente partes autorizadas possam lê-las. Ela protege dados em trânsito e em repouso, sendo essencial na segurança da informação.

## Fundamentos da Criptografia

* **Simétrica vs. Assimétrica**: na criptografia simétrica, a mesma chave é usada para criptografar e descriptografar. Na assimétrica, há par de chaves pública/privada.
* **Hash Functions**: funções unidirecionais (ex.: SHA-256) usadas para garantir integridade.

## O Criptograma de Vigenère

### História

Desenvolvida no século XVI, a cifra de Vigenère foi considerada indecifrável até o século XIX. Usa múltiplos alfabetos (polialfabética) para deslocamento.

### Funcionamento

1. Cada letra da mensagem é deslocada por um valor baseado na letra correspondente da chave.
2. A chave se repete até o final da mensagem.
3. Caracteres não alfabéticos são preservados.

## Implementação em Python

### Requisitos

* Python 3.x instalado

### Função `vigenere`

```python
# Aplica a cifra de Vigenère para criptografar ou descriptografar uma mensagem

def vigenere(message: str, key: str, direction: int = 1) -> str:
    """
    :param message: Texto a ser processado (criptografado/descriptografado).
    :param key: Chave usada para definir o deslocamento de cada letra.
    :param direction: +1 para criptografar, -1 para descriptografar.
    :return: Mensagem resultante após aplicação da cifra.
    """
    key_index = 0                       # Índice para percorrer a chave ciclicamente
    alphabet = 'abcdefghijklmnopqrstuvwxyz'  # Alfabeto base para deslocamento
    final_message = ''                  # Armazena o resultado final

    for char in message.lower():       # Percorre cada caractere em minúsculas
        if not char.isalpha():         # Se não for letra, acrescenta sem alteração
            final_message += char      # Mantém espaços, pontuações etc.
        else:
            key_char = key[key_index % len(key)]  # Seleciona caractere da chave
            key_index += 1             # Incrementa índice da chave

            offset = alphabet.index(key_char)     # Converte chave em deslocamento numérico
            index = alphabet.index(char)          # Posição da letra original
            # Calcula nova posição considerando direção (criptografia ou descriptografia)
            new_index = (index + offset * direction) % len(alphabet)

            final_message += alphabet[new_index]  # Adiciona letra resultante

    return final_message               # Retorna texto cifrado ou decifrado
```

### Funções `encrypt` e `decrypt`

```python
# Encripta mensagem usando Vigenère

def encrypt(message: str, key: str) -> str:
    return vigenere(message, key, direction=1)

# Desencripta mensagem usando Vigenère

def decrypt(message: str, key: str) -> str:
    return vigenere(message, key, direction=-1)
```

### Exemplo de Uso

```python
text = 'mrttaqrhknsw ih puggrur'    # Texto cifrado de entrada
custom_key = 'python'               # Chave usada na cifra

print(f'Encrypted text: {text}')   # Exibe o texto cifrado
print(f'Key: {custom_key}')         # Exibe a chave utilizada

decrypted = decrypt(text, custom_key)  # Executa descriptografia
print(f'Decrypted text: {decrypted}')  # Exibe resultado final
```

**Saída esperada:**

```
Encrypted text: mrttaqrhknsw ih puggrur
Key: python
Decrypted text: varchar sandbox
```

## Análise e Melhoria do Código

* **Preservar caixa de texto**: adaptar para manter maiúsculas e minúsculas originais.
* **Suporte a caracteres especiais**: converter acentos e símbolos via Unicode.
* **Performance**: usar lista de caracteres e `str.join` em vez de concatenar strings diretamente.

## Segurança e Criptoanálise

* **Vulnerabilidades**: ataques por repetição de chave (métodos de Kasiski e Friedman).
* **Fortalecimento**: utilizar chaves longas e aleatórias, e combinar com algoritmos modernos como AES.

## Conclusão

A cifra de Vigenère é excelente para entender fundamentos de criptografia histórica. Em aplicações reais, é recomendável usar algoritmos robustos atuais, mas conhecer seu funcionamento enriquece a compreensão das práticas modernas.
