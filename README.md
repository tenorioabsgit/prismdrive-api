# 📂 Gerenciador de Arquivos - Prism Drive API

Este projeto permite realizar **upload, download, criação e remoção de arquivos e pastas** utilizando a **API Prism Drive**. Através do **R**, podemos interagir com o serviço de armazenamento em nuvem e automatizar o gerenciamento de arquivos.

## 📌 Funcionalidades

✅ **Autenticação via API**  
✅ **Criação de pastas na nuvem**  
✅ **Upload e download de arquivos**  
✅ **Upload de todos os arquivos dentro de uma pasta**  
✅ **Deleção de arquivos e pastas**  

---

## 🛠 Tecnologias Utilizadas

- **Linguagem:** R  
- **Pacotes:** `httr`, `curl`  
- **API Utilizada:** [Prism Drive](https://app.prismdrive.com/api/v1)  

---

## 📂 Estrutura do Código

```
📁 /  (Diretório Raiz)
├── 00_funcoes_e_autenticacao.R   # Funções para autenticação e interação com a API
├── 01_main.R                     # Script principal para execução das funções
```

---

## 🚀 Como Executar

### 1️⃣ **Instalar os pacotes necessários**

Se ainda não estiverem instalados, execute no R:

```r
install.packages(c("httr", "curl"))
```

### 2️⃣ **Configurar Credenciais**

No arquivo `00_funcoes_e_autenticacao.R`, defina **seu e-mail e senha** para autenticar na API:

```r
token <- authenticate("SEU_EMAIL_AQUI", "SUA_SENHA_AQUI")
```

### 3️⃣ **Criar uma Pasta na Nuvem**

Para criar uma pasta, rode o seguinte código:

```r
resultado_pasta <- create_folder("Minha_Pasta")
folder_id <- resultado_pasta$folder$id
print(folder_id)
```

### 4️⃣ **Fazer Upload de Arquivos**

Para fazer o upload de um arquivo, use:

```r
upload_file_curl("caminho/do/arquivo.txt", parent_id = folder_id)
```

Para fazer **upload de todos os arquivos dentro de uma pasta**, use:

```r
upload_folder("caminho/da/pasta", parent_id = folder_id)
```

### 5️⃣ **Fazer Download de um Arquivo**

```r
download_file(file_id = "ID_DO_ARQUIVO", save_path = "caminho/local/arquivo.txt")
```

### 6️⃣ **Deletar Arquivos ou Pastas**

```r
delete_entry(entry_id = "ID_DO_ARQUIVO_OU_PASTA")
```

---

## 🛑 Possíveis Problemas e Soluções

### ⚠️ **Erro de Autenticação**
Se a autenticação falhar, verifique se **o e-mail e a senha estão corretos** e se a API está funcionando corretamente.

### ⏳ **Upload muito lento?**
Se estiver enviando **muitos arquivos**, pode ser útil dividir os uploads em blocos menores.

---

## 👨‍💻 Autor

👤 **José Tenório Abs Junior**  
📧 [Seu Email]  
🏛 **IBPAD - Instituto Brasileiro de Pesquisa e Análise de Dados**  

🚀 **Agora você pode gerenciar seus arquivos na nuvem!** Qualquer dúvida, me avise! 😊
