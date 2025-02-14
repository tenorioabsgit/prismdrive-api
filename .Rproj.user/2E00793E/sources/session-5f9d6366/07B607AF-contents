library(curl)
library(httr)

api_url  <- "https://app.prismdrive.com/api/v1"

authenticate <- function(username, password) {
  response <- POST(
    url = paste0(api_url , "/auth/login"),
    body = list(email = username, password = password, token_name = "my_token"),
    encode = "json"
  )
  
  response_content <- content(response, "parsed")
  if (!is.null(response_content$user$access_token)) {
    token <- response_content$user$access_token
    return(token)
  } else {
    stop("Failed to retrieve token: ", response_content$message)
  }
}

token <- authenticate("email@gmail.com", "senha")
print(token)

# Configuração do cabeçalho de autorização
auth_header <- add_headers(Authorization = paste("Bearer", token))

# Função para criar uma pasta
create_folder <- function(name, parent_id = NULL) {
  url <- paste0(api_url, "/folders")
  body <- list(name = name, parentId = parent_id)
  response <- POST(url, auth_header, body = body, encode = "json")
  content(response, "parsed")
}

# Função para deletar uma pasta ou arquivo
delete_entry <- function(entry_id) {
  url <- paste0(api_url, "/file-entries/", entry_id)
  response <- DELETE(url, auth_header)
  content(response, "parsed")
}

# Função para upload de arquivo revisada
upload_file_curl <- function(file_path, parent_id = NULL) {
  url <- paste0(api_url, "/uploads")
  
  # Preparar o formulário com o arquivo
  form_data <- list(
    file = curl::form_file(file_path),
    parentId = parent_id
  )
  
  # Configurar e realizar a requisição POST
  response <- httr::POST(url, httr::add_headers(Authorization = paste("Bearer", token)), 
                         body = form_data, encode = "multipart")
  
  # Processar a resposta
  if (response$status_code == 200 || response$status_code == 201) {
    httr::content(response)
  } else {
    stop("Erro na requisição: ", httr::content(response, "text"))
  }
}

# Função para download de arquivo
download_file <- function(file_id, save_path) {
  url <- paste0(api_url, "/file-entries/", file_id, "/download")
  response <- GET(url, auth_header, write_disk(save_path, overwrite = TRUE))
  response$status_code
}

# Função para fazer o upload de todos os arquivos dentro de uma pasta
upload_folder <- function(folder_path, parent_id = NULL) {
  # Listar todos os arquivos na pasta
  files <- list.files(folder_path, full.names = TRUE, recursive = TRUE)
  
  # Verificar se existem arquivos
  if (length(files) == 0) {
    cat("Nenhum arquivo encontrado na pasta.\n")
    return(NULL)
  }
  
  # Iterar sobre cada arquivo e fazer o upload
  results <- lapply(files, function(file) {
    url <- paste0(api_url, "/uploads")
    # Preparar o corpo da requisição
    # Nota: 'upload_file()' é uma função do 'httr' para preparar arquivos para upload
    body <- list(file = upload_file(file), parentId = parent_id)
    
    # Fazer a requisição POST
    response <- POST(url, add_headers(Authorization = paste("Bearer", token)),
                     body = body, encode = "multipart")
    
    # Processar a resposta
    if (response$status_code >= 200 && response$status_code < 300) {
      return(content(response))
    } else {
      cat("Falha no upload do arquivo:", file, "\nStatus code:", response$status_code, "\n")
      return(list(status_code = response$status_code, content = content(response)))
    }
  })
  
  return(results)
}