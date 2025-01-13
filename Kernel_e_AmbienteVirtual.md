
# Guia para Entender Kernel e Ambiente Virtual

- poetry env list
- poetry env info
- jupyter kernelspec list

## **1. Ambiente Virtual (Poetry)**
Um ambiente virtual é uma instalação isolada do Python para gerenciar dependências de projetos sem interferir em outros.

### **No contexto do Poetry:**
- O Poetry cria e gerencia ambientes virtuais automaticamente.
- Ele garante que as dependências sejam específicas para cada projeto.
- Comando para listar ambientes criados pelo Poetry:
  ```bash
  poetry env list
  ```
- Exemplo de saída:
  ```
  brazilian-e-commerce-project-WFljCIXp-py3.11 (Activated)
  ```

### **Vantagens:**
- Evita conflitos de versões entre projetos.
- Permite gerenciar dependências de forma eficiente.

---

## **2. Kernel (Jupyter Notebook)**
O kernel é o processo que executa o código dentro de notebooks Jupyter.

### **Características do Kernel:**
- Cada kernel está associado a um ambiente virtual.
- Permite rodar código Python interativamente no Jupyter.
- Você pode registrar um ambiente virtual como kernel no Jupyter.

### **Comando para registrar o ambiente virtual como kernel:**
```bash
python -m ipykernel install --user --name=nome_do_kernel --display-name "Nome Amigável"
```
- **Exemplo:**
  ```bash
  python -m ipykernel install --user --name=ambiente_exploratorio --display-name "Python (minha_venv)"
  ```

### **Listar kernels disponíveis:**
```bash
jupyter kernelspec list
```
- Exemplo de saída:
  ```
  Available kernels:
    ambiente_exploratorio    /home/jovyan/.local/share/jupyter/kernels/ambiente_exploratorio
    python3                  /opt/conda/share/jupyter/kernels/python3
  ```

---

## **Diferença entre Kernel e Ambiente Virtual**
| **Ambiente Virtual (Poetry)**             | **Kernel (Jupyter Notebook)**           |
|-------------------------------------------|-----------------------------------------|
| Isola pacotes e dependências de um projeto.| Executa código no Jupyter Notebook.     |
| Criado pelo Poetry, `venv` ou `conda`.    | Registrado com `ipykernel`.             |
| Funciona em terminais ou IDEs.            | Específico para Jupyter Notebook/Lab.   |
| Gerencia dependências de projetos.        | Vinculado a um ambiente virtual.        |

---

## **Resumo do Processo**
1. Crie um ambiente virtual com o Poetry:
   ```bash
   poetry init
   poetry install
   ```
2. Ative o ambiente virtual:
   ```bash
   poetry shell
   ```
3. Registre o ambiente como kernel no Jupyter:
   ```bash
   python -m ipykernel install --user --name=ambiente_exploratorio --display-name "Python (minha_venv)"
   ```
4. Liste e selecione o kernel no Jupyter Notebook.

---

Esperamos que este guia tenha ajudado a esclarecer as diferenças e o uso de ambientes virtuais e kernels no Jupyter Notebook. Para dúvidas, entre em contato!
