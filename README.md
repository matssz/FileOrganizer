# FileOrganizer
Script Python que organiza automaticamente os arquivos de uma pasta com base na extensão (como .pdf, .jpg, .docx, .mp3 etc.). 


# FileOrganizer - Organizador Automático de Arquivos
# Linguagem: Python 3.x

import os
import shutil
from datetime import datetime

# Mapear categorias de arquivos
CATEGORIAS = {
    "Imagens": [".jpg", ".jpeg", ".png", ".gif", ".bmp"],
    "Documentos": [".pdf", ".doc", ".docx", ".txt", ".xls", ".xlsx", ".ppt", ".pptx"],
    "Músicas": [".mp3", ".wav", ".aac"],
    "Vídeos": [".mp4", ".avi", ".mkv", ".mov"],
    "Compactados": [".zip", ".rar", ".7z", ".tar"],
    "Executáveis": [".exe", ".msi"],
    "Códigos": [".py", ".js", ".html", ".css", ".c", ".cpp", ".java"]
}

PASTA_ORIGEM = os.path.expanduser("~/Downloads")
PASTA_DESTINO = os.path.expanduser("~/Downloads/Organizado")


def criar_pastas():
    if not os.path.exists(PASTA_DESTINO):
        os.makedirs(PASTA_DESTINO)
    for categoria in CATEGORIAS:
        caminho = os.path.join(PASTA_DESTINO, categoria)
        if not os.path.exists(caminho):
            os.makedirs(caminho)
    caminho_outros = os.path.join(PASTA_DESTINO, "Outros")
    if not os.path.exists(caminho_outros):
        os.makedirs(caminho_outros)


def organizar():
    arquivos = os.listdir(PASTA_ORIGEM)
    for arquivo in arquivos:
        caminho_completo = os.path.join(PASTA_ORIGEM, arquivo)

        if os.path.isfile(caminho_completo):
            extensao = os.path.splitext(arquivo)[1].lower()
            movido = False

            for categoria, extensoes in CATEGORIAS.items():
                if extensao in extensoes:
                    destino = os.path.join(PASTA_DESTINO, categoria, arquivo)
                    shutil.move(caminho_completo, destino)
                    movido = True
                    print(f"Movido: {arquivo} para {categoria}")
                    break

            if not movido:
                destino = os.path.join(PASTA_DESTINO, "Outros", arquivo)
                shutil.move(caminho_completo, destino)
                print(f"Movido: {arquivo} para Outros")


if __name__ == "__main__":
    print("Organizando arquivos...")
    criar_pastas()
    organizar()
    print("Organização concluída!")
