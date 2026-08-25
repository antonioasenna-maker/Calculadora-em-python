"""
Calculadora com Interface Gráfica (Tkinter)
"""

import tkinter as tk

# ----------------- Funções -----------------

def inserir(valor):
    display.insert(tk.END, valor)


def limpar():
    display.delete(0, tk.END)


def apagar_ultimo():
    conteudo = display.get()
    display.delete(0, tk.END)
    display.insert(0, conteudo[:-1])


def calcular():
    try:
        expressao = display.get()
        # Substitui símbolos visuais pelos operadores do Python
        expressao = expressao.replace("×", "*").replace("÷", "/").replace(",", ".")
        resultado = eval(expressao)
        display.delete(0, tk.END)
        display.insert(0, str(resultado))
    except ZeroDivisionError:
        display.delete(0, tk.END)
        display.insert(0, "Erro: divisão por 0")
    except Exception:
        display.delete(0, tk.END)
        display.insert(0, "Erro")


# ----------------- Interface -----------------

janela = tk.Tk()
janela.title("Calculadora")
janela.resizable(False, False)
janela.configure(bg="#1e1e1e")

# Display
display = tk.Entry(
    janela,
    font=("Arial", 24),
    justify="right",
    bd=0,
    bg="#2b2b2b",
    fg="white",
    insertbackground="white",
)
display.grid(row=0, column=0, columnspan=4, ipady=20, sticky="nsew", padx=5, pady=5)

# Configuração dos botões: (texto, linha, coluna, cor_bg, ação)
botoes = [
    ("C", 1, 0, "#a83232", limpar),
    ("⌫", 1, 1, "#a83232", apagar_ultimo),
    ("÷", 1, 2, "#690e8d", lambda: inserir("÷")),
    ("×", 1, 3, "#690e8d", lambda: inserir("×")),

    ("7", 2, 0, "#3a3a3a", lambda: inserir("7")),
    ("8", 2, 1, "#3a3a3a", lambda: inserir("8")),
    ("9", 2, 2, "#3a3a3a", lambda: inserir("9")),
    ("-", 2, 3, "#690e8d", lambda: inserir("-")),

    ("4", 3, 0, "#3a3a3a", lambda: inserir("4")),
    ("5", 3, 1, "#3a3a3a", lambda: inserir("5")),
    ("6", 3, 2, "#3a3a3a", lambda: inserir("6")),
    ("+", 3, 3, "#690e8d", lambda: inserir("+")),

    ("1", 4, 0, "#3a3a3a", lambda: inserir("1")),
    ("2", 4, 1, "#3a3a3a", lambda: inserir("2")),
    ("3", 4, 2, "#3a3a3a", lambda: inserir("3")),
    ("=", 4, 3, "#32a852", calcular),

    ("0", 5, 0, "#3a3a3a", lambda: inserir("0")),
    (",", 5, 1, "#3a3a3a", lambda: inserir(",")),
    ("(", 5, 2, "#3a3a3a", lambda: inserir("(")),
    (")", 5, 3, "#3a3a3a", lambda: inserir(")")),
]

for (texto, linha, coluna, cor, comando) in botoes:
    btn = tk.Button(
        janela,
        text=texto,
        font=("Arial", 18),
        bg=cor,
        fg="white",
        activebackground="#555555",
        bd=0,
        command=comando,
    )
    btn.grid(row=linha, column=coluna, sticky="nsew", ipadx=10, ipady=15, padx=3, pady=3)

# Faz as colunas/linhas expandirem igualmente
for i in range(4):
    janela.columnconfigure(i, weight=1)
for i in range(6):
    janela.rowconfigure(i, weight=1)

# Permite usar o teclado também
def tecla_pressionada(evento):
    tecla = evento.char
    if tecla in "0123456789+-()":
        inserir(tecla)
    elif tecla == "*":
        inserir("×")
    elif tecla == "/":
        inserir("÷")
    elif tecla in (",", "."):
        inserir(",")
    elif evento.keysym == "Return":
        calcular()
    elif evento.keysym == "BackSpace":
        apagar_ultimo()
    elif tecla.lower() == "c":
        limpar()

janela.bind("<Key>", tecla_pressionada)

janela.mainloop()
