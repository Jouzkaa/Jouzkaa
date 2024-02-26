import requests
import json
import os

def create_github_repo(repo_name, token):
    headers = {
        'Authorization': f'token {token}',
        'Accept': 'application/vnd.github.v3+json'
    }
    data = {'name': repo_name, 'auto_init': True}
    response = requests.post('https://api.github.com/user/repos', headers=headers, data=json.dumps(data))
    if response.status_code == 201:
        print(f"Repository '{repo_name}' berhasil dibuat di GitHub!")
        return True
    else:
        print(f"Failed to create repository. Status code: {response.status_code}")
        return False

def push_to_github(repo_name, file_path, token):
    headers = {
        'Authorization': f'token {token}',
        'Accept': 'application/vnd.github.v3+json'
    }
    with open(file_path, 'r') as file:
        content = file.read()
    data = {'message': 'Add new file', 'content': content}
    response = requests.put(f'https://api.github.com/repos/yourusername/{repo_name}/contents/{file_path}', headers=headers, data=json.dumps(data))
    if response.status_code == 201:
        print(f"File '{file_path}' berhasil di-upload ke GitHub!")
        return True
    else:
        print(f"Failed to upload file. Status code: {response.status_code}")
        return False

# Gunakan token GitHub Anda
github_token = 'YOUR_GITHUB_TOKEN'
# Ganti dengan nama repository yang ingin Anda buat
repository_name = 'test-repo'
# Ganti dengan nama file yang ingin Anda unggah
file_to_upload = 'example.txt'

# Membuat repository baru
if create_github_repo(repository_name, github_token):
    # Melakukan komit file ke repository
    if push_to_github(repository_name, file_to_upload, github_token):
        print("Proses berhasil!")
    else:
        print("Proses gagal!")
else:
    print("Proses gagal!")
