# aula-ter-a-feira-

A parte final do codigo:


import os

#######################################
# Configurações do sistema
Média_para_passar = 5.0
Frequencia_para_passar = 75.0
aulas_totais = 200

# LISTA: Armazena dados fixos que o sistema usa para validar as entradas
Disciplinas_Permitidas = ["Matemática", "Português", "História", "Geografia"]

# LISTA GLOBAL: Vai guardar os dicionários de cada aluno cadastrado
alunos = []
#######################################


# Executando o programa
def menu_principal():
    """Menu principal do sistema acadêmico"""
    print("\nBem-vindo ao Sistema Acadêmico!!\n")

    # ESTRUTURA DE REPETIÇÃO: Mantém o menu ativo
    while True:
        print("\n" + "="*50)
        print("MENU PRINCIPAL")
        print("="*50)
        print("1. Cadastrar aluno")
        print("2. Registrar nota por disciplina")
        print("3. Registrar faltas por disciplina")
        print("4. Verificar situação do aluno")
        print("5. Sair")
        print("="*50)

        escolha = input("Escolha uma opção (1-5): ").strip()

        # ESTRUTURA DE DECISÃO ENCADEADA: Menu de opções
        if escolha == "1":
            cadastrar_aluno()
        elif escolha == "2":
            registrar_nota()
        elif escolha == "3":
            registrar_faltas()
        elif escolha == "4":
            verificar_situacao()
        elif escolha == "5":
            print("Encerrando o sistema. Até logo!")
            break 
        else:
            print("Opção inválida! Tente novamente.")



# Funções do sistema

def cadastrar_aluno():
    """Cadastra um novo aluno no sistema"""
    # REPETIÇÃO: Validação de nome
    while True:
        nome = input("Digite o nome completo do aluno: ").strip()
        # DECISÃO: Se o nome não for vazio, sai do loop
        if nome != "":
            break
        print("O campo nome não pode estar vazio")
    
    serie = input("Série/ano do aluno (exemplo: 1º ano): ")

    # DICIONÁRIO: Estrutura de dados do aluno
    aluno = {
        "nome": nome,
        "serie": serie,
        "disciplinas": {
            "Matemática": {"bimestres": [0, 0, 0, 0], "faltas": 0},
            "Português": {"bimestres": [0, 0, 0, 0], "faltas": 0},
            "História": {"bimestres": [0, 0, 0, 0], "faltas": 0},
            "Geografia": {"bimestres": [0, 0, 0, 0], "faltas": 0}
        }
    }
    
    # MANIPULAÇÃO DE LISTA: Adiciona o aluno à lista global
    alunos.append(aluno)
    print(f"Aluno {nome} cadastrado com sucesso!")


def registrar_nota():
    """Registra as notas dos 4 bimestres para um aluno em uma disciplina"""
    nome = input("Digite o nome completo do aluno: ").strip()
    
    aluno_encontrado = None
    # REPETIÇÃO + DECISÃO: Busca o aluno na lista
    for a in alunos:
        if a["nome"].lower() == nome.lower():
            aluno_encontrado = a
            break
    
    # DECISÃO SEQUENCIAL: Validação de existência
    if not aluno_encontrado:
        print("Aluno não encontrado. Por favor, cadastre o aluno primeiro.")
        return

    print(f"Disciplinas disponíveis: {Disciplinas_Permitidas}")
    disciplina = input("Digite a disciplina: ").strip()
    
    # DECISÃO: Valida se a disciplina existe na lista
    if disciplina not in Disciplinas_Permitidas:
        print("Disciplina inválida!")
        return

    # LISTA LOCAL: Notas do momento
    notas_bimestres = []
    
    # REPETIÇÃO CONTROLADA: Executa 4 vezes para os 4 bimestres
    for bimestre in range(1, 5): 
        # REPETIÇÃO: Tratamento de erro na digitação da nota
        while True:
            try:
                nota = float(input(f"Digite a nota de {disciplina} no {bimestre}º bimestre (0-10): "))
                
                # DECISÃO: Valida o intervalo numérico da nota
                if 0 <= nota <= 10:
                    notas_bimestres.append(nota) 
                    print(f"✓ Nota {nota} registrada!")
                    break
                else:
                    print("A nota deve ser entre 0 e 10!")
            except ValueError:
                print("Erro: digite um número válido!")
    
    # MANIPULAÇÃO DE DICIONÁRIO: Atualiza as notas no cadastro do aluno
    aluno_encontrado["disciplinas"][disciplina]["bimestres"] = notas_bimestres
    
    # ESTRUTURA SEQUENCIAL: Cálculos e exibição final
    total = sum(notas_bimestres)
    print(f"\n--- RESUMO ---")
    print(f"Disciplina: {disciplina}")
    print(f"1º Bimestre: {notas_bimestres[0]}") 
    print(f"2º Bimestre: {notas_bimestres[1]}")
    print(f"3º Bimestre: {notas_bimestres[2]}")
    print(f"4º Bimestre: {notas_bimestres[3]}")
    print(f"TOTAL DE PONTOS: {total}")
    print(f"\n ATENÇÃO:")
    print(f" • Mínimo para aprovação: 20 pontos")
    print(f" • Frequência mínima: 75%")
    # OPERADOR TERNÁRIO: Decisão em linha
    print(f" • Seu situação atual: {'✓ APROVADO' if total >= 20 else '✗ REPROVADO (pontos insuficientes)'}")


def registrar_faltas():
    #Registra faltas para um aluno em uma disciplina
    nome = input("Digite o nome completo do aluno: ").strip()
    
    # BUSCA DE DADOS: Começa assumindo que não achou o aluno
    aluno_encontrado = None

    # ESTRUTURA DE REPETIÇÃO: Procura o aluno na lista usando um loop
    for a in alunos:
        # ESTRUTURA DE DECISÃO: Se achar o nome (ignorando maiúsculas/minúsculas)
        if a["nome"].lower() == nome.lower():
            aluno_encontrado = a
            break
    # ESTRUTURA DE DECISÃO SEQUENCIAL: Se o aluno continuou como 'None' (não achou)
    if not aluno_encontrado:
        print("Aluno não encontrado.")
        return

    print(f"Disciplinas disponíveis: {Disciplinas_Permitidas}")
    disciplina = input("Digite a disciplina: ").strip()
    
    if disciplina not in Disciplinas_Permitidas:
        print("Disciplina inválida!")
        return

    # REPETIÇÃO: Loop de validação de número inteiro
    while True:
        try:
            faltas = int(input(f"Digite o número de faltas em {disciplina}: "))
            # DECISÃO: Valida número negativo
            if faltas >= 0:
                break
            else:
                print("O número de faltas não pode ser negativo!")
        except ValueError:
            print("Erro: digite um número inteiro válido!")

    # OPERAÇÃO SEQUENCIAL: Incrementa o valor no dicionário
    aluno_encontrado["disciplinas"][disciplina]["faltas"] += faltas
    print(f"{faltas} faltas registradas em {disciplina} para {nome}.")


def verificar_situacao():
    """Verifica e exibe a situação acadêmica de um aluno"""
    nome = input("Digite o nome completo do aluno: ").strip()

    aluno_encontrado = None
    for a in alunos:
        if a["nome"].lower() == nome.lower():
            aluno_encontrado = a
            break

    if not aluno_encontrado:
        print("Aluno não encontrado.")
        return

    print(f"\n--- SITUAÇÃO DO ALUNO: {aluno_encontrado['nome']} ({aluno_encontrado['serie']}) ---")
    print(f"{'Disciplina':<20} | {'1º Bim':<8} | {'2º Bim':<8} | {'3º Bim':<8} | {'4º Bim':<8} | {'TOTAL':<8} | {'Freq%':<8} | {'Situação':<12}")
    print("-" * 115)

    # REPETIÇÃO: Varre as chaves e valores do dicionário de disciplinas
    for disciplina, dados in aluno_encontrado["disciplinas"].items():
        bimestres = dados["bimestres"]
        faltas = dados["faltas"]

        # ESTRUTURA SEQUENCIAL: Processamento lógico e matemático dos dados
        total = sum(bimestres)
        frequencia = ((aulas_totais - faltas) / aulas_totais) * 100

        # ESTRUTURA DE DECISÃO ENCADEADA: Classificação final da situação do aluno
        if frequencia < Frequencia_para_passar:
            situacao = "Rep. Faltas"
        elif total >= 20:
            situacao = "Aprovado"
        else:
            situacao = "Rep. Nota"

        # SEQUENCIAL: Desempacotamento de lista para variáveis individuais
        bim1, bim2, bim3, bim4 = bimestres
        print(f"{disciplina:<20} | {bim1:<8.1f} | {bim2:<8.1f} | {bim3:<8.1f} | {bim4:<8.1f} | {total:<8.1f} | {frequencia:<8.1f} | {situacao:<12}")



if __name__ == "__main__":
    menu_principal()