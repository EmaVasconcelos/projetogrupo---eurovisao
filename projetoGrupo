import time
import random
from datetime import datetime;time
from collections import defaultdict
import os
import csv


# Definir cores para os títulos e mensagens para o utilizador,de forma a ficar mais organizado
class Cores:

    RESET = '\033[0m'
    VERMELHO = '\033[31m'
    VERDE = '\033[32m'
    AMARELO = '\033[33m'
    AZUL = '\033[34m'
    CIANO = '\033[36m'
    NEGRITO = '\033[1m'

    BG_VERMELHO='\033[33;31m'
    BG_VERDE='\033[33;32m'
    BG_AMARELO='\033[33;33m'
    BG_AZUL='\033[33;44m'  
    BG_CIANO='\033[33;36m'


# Função limpar ecra

def limpar_ecra():
    os.system('cls' if os.name == 'nt' else 'clear' )


# Função digitar qualquer tecla (pausa para utlizador)

def pressione_tecla():
    input(f"\n{Cores.AMARELO}[qualquer tecla para voltar atrás]{Cores.RESET}")   

# Função para validar a data

def validar_data(data_in):

    try:

        return datetime.strptime(data_in, "%d-%m-%Y")

    except ValueError:

        print("Data inválida. Por favor, introduza no formato DD-MM-AAAA.")
        return None


# Função para validar a hora

def validar_hora(hora_in):

    try:

        return datetime.strptime(hora_in, "%H:%M").time()

    except ValueError:

        print("Hora inválida. Por favor, introduza no formato HH:MM.")
        return None


# Gerar chave única para eleitores

def key():

    timestamp = int(datetime.now().toordinal())
    random_number = random.randint(10000, 99999)
    key = f"{timestamp}-{random_number}"
    return key


# Função gravar votação do Juri

def gravar_votacao_juri(jurado, votos_registrados):

    #Grava a votação de um jurado em um arquivo CSV.

    ficheiro_csv = "votacaoJuri.csv"

    # Verificar se o arquivo já existe

    cabecalho = ["IdKey", "País Votado", "Pontos"]

    modo = "a" if os.path.exists(ficheiro_csv) else "w"


    with open(ficheiro_csv, mode=modo, newline="", encoding="utf-8") as ficheiro:
        streamWriter = csv.DictWriter(ficheiro, fieldnames=cabecalho)

        # Escrever o cabeçalho apenas se for um novo arquivo
        if modo == "w":
            streamWriter.writeheader()

        # Gravar os votos

        for voto in votos_registrados:
            streamWriter.writerow({"IdKey": jurado, "País Votado": voto["pais"], "Pontos": voto["pontos"]})


    print(f"{Cores.VERDE}Votação de {jurado} gravada com sucesso!{Cores.RESET}")




# Função Geral para escrever ficheiro csv

def gravar_dados(dados, tabela):


    match tabela:

        case "definicoes":
            with open("definicoes.csv", mode="w", newline="", encoding="utf-8") as ficheiro:

                streamWriter = csv.DictWriter(ficheiro, fieldnames=dados.keys())
                streamWriter.writeheader()
                streamWriter.writerow(dados)


        case "participantes":

            with open("participantes.csv", mode='w', newline='', encoding='utf-8') as ficheiro:

                streamWriter =  csv.DictWriter(ficheiro, fieldnames=['IdKey','nome', 'idade', 'pais', 'musica', 'tempo_musica'])
                streamWriter.writeheader()
                streamWriter.writerows(dados)

                     

        case "concurso":

            print("Não implementado ainda")

        case "juri":

             print("Não implementado ainda")        

                   

# Função Geral para escrever ficheiro csv

def carregar_dados(tabela):

    match tabela:

        case "definicoes":

            if not os.path.exists("definicoes.csv"):

                print("\Ficheiro não encontrado. Nenhum dado disponível.")

                return None

            with open("definicoes.csv", mode="r", encoding="utf-8") as ficheiro:

                streamReader = csv.DictReader(ficheiro)

                for linha in streamReader:

                    return linha

            return None

        case "participantes":

            try:

                with open("participantes.csv", mode='r', encoding='utf-8') as ficheiro:

                    streamReader = csv.DictReader(ficheiro)

                    return [

                            {

                            'IdKey': linha['IdKey'],
                            'nome': linha['nome'],
                            'idade': int(linha['idade']),
                            'pais': linha['pais'],
                            'musica': linha['musica'],
                            'tempo_musica': float(linha['tempo_musica'])

                            }

                            for linha in streamReader 

                            ]

            except FileNotFoundError:

                return []

        case _:

            return []

        

#Alterar definições do sistema

def alterar_definicoes(dados):


    if not dados:

        print("\nNenhum dado encontrado para alterar. Você deve criar um registro primeiro.")

        return

    

    print(f"{Cores.BG_AZUL}[Alteração de Dados]{Cores.RESET}\n")

    for campo in dados.keys():

        if campo in ["Data do Concurso"]:

            while True:

                novo_valor = input(f"Novo valor para {campo} (Atual: {dados[campo]}): ")

                if validar_data(novo_valor):

                    dados[campo] = novo_valor

                    break

        elif campo in ["Hora Inicio", "Hora Fim"]:

            while True:

                novo_valor = input(f"Novo valor para {campo} (Atual: {dados[campo]}): ")

                if validar_hora(novo_valor):

                    dados[campo] = novo_valor

                    break

        else:

            novo_valor = input(f"Novo valor para {campo} (Atual: {dados[campo]}): ")

            if novo_valor:

                dados[campo] = novo_valor

    

    gravar_dados(dados, "definicoes")

    input(f"\n{Cores.AMARELO}[qualquer tecla para voltar atrás]{Cores.RESET}")

    limpar_ecra()        


#Consultar definições do sistema 

def consultar_definicoes(dados):

    if not dados:

        print(f"{Cores.BG_AMARELO}Ainda não tem definições.{Cores.RESET}")

        return

    else:

        print(f"\n{Cores.BG_AZUL}[Definições do sistema]\n{Cores.RESET}")
        print(f"Numero Maximo de Participantes: {int(dados['Numero Maximo de Participantes'])}")
        print(f"Nome do Concurso: {dados['Nome do Concurso']}")
        print(f"Data do Concurso: {dados['Data do Concurso']}")
        print(f"Hora Inicio: {dados['Hora Inicio']}")
        print(f"Hora Fim: {dados['Hora Fim']}\n")
        input(f"\n{Cores.AMARELO}[qualquer tecla para voltar atrás]{Cores.RESET}")

        limpar_ecra()

    

# Carregar participantes de arquivo CSV

def carregar_participantes_csv(arquivo_csv="participantes.csv"):

    try:

        with open(arquivo_csv, mode='r', encoding='utf-8') as arquivo:

            leitor = csv.DictReader(arquivo)

            return [

                {

                    'IdKey': linha['IdKey'],
                    'nome': linha['nome'],
                    'idade': int(linha['idade']),
                    'pais': linha['pais'],
                    'musica': linha['musica'],
                    'tempo_musica': float(linha['tempo_musica'])

                }

                for linha in leitor

            ]

    except FileNotFoundError:

        return []


def votacao_juri():

    global participantes
    global votos 


    # Verificar se o concurso já terminou

    if data_processamento < data_concurso or (data_processamento == data_concurso and hora_processamento < hora_fim.time()):

        print(f"{Cores.AMARELO}O concurso ainda não terminou! A votação do júri só acontece após o termino do concurso.{Cores.RESET}")
        pressione_tecla()

        return

    

    total_participantes = len(participantes)


    for jurado in participantes:

        print(f"\n--- Votação do Júri de {jurado['pais']} ---")

        # Pontuação em ordem decrescente

        pontuacoes = list(range(total_participantes, 1, -1))
        votos_validos = []
        votos_registrados = []


        while pontuacoes:

            print(f"Pontuações restantes: {pontuacoes}")
            print(f"Países disponíveis: {[pais['pais'] for pais in participantes if pais['pais'] not in votos_validos and pais['pais'] != jurado['pais']]}")
            voto = input(f"Júri de {jurado['pais']}, escolha um país para dar {pontuacoes[0]} pontos (não pode votar no próprio país): ")

            

            pais_existe = any(item['pais'] == voto for item in participantes)

            if not pais_existe:

                print("País inválido. Tente novamente.")

            elif voto == jurado['pais']:

                print("Você não pode votar no próprio país. Tente novamente.")

            elif voto in votos_validos:

                print("Você já votou neste país. Tente novamente.")

            else:

                # Registrar o voto

                pontos = pontuacoes.pop(0)
                votos[voto] += pontos
                votos_validos.append(voto)
                votos_registrados.append({"pais": voto, "pontos": pontos})
                print(f"Voto registrado: {voto} recebeu {votos[voto]} pontos.")

                

        # Gravar os votos do jurado

        gravar_votacao_juri(jurado['IdKey'], votos_registrados)


        print(f"Votação do Júri de {jurado['pais']} concluída!")


#Votação do Publico
#Função para registar a votação do público.
#Pede uma identificação (número de telefone ou país), valida as condições de idade
#e impede votos no próprio país. Os resultados são gravados em 'votacaoPublico.csv'
#e os dados do eleitor são registados separadamente em 'eleitoresPublico.csv'.

def votacao_publico():

    limpar_ecra()

    print(f"{Cores.BG_AZUL}[--- Votação do Público ---]{Cores.RESET}\n")

    

    ficheiro_votos = "votacaoPublico.csv"

    ficheiro_eleitores = "eleitoresPublico.csv"


    # Verificar existência de participantes

    if not participantes:

        print(f"{Cores.AMARELO}Ainda não há participantes registados! A votação não pode ser realizada.{Cores.RESET}")

        pressione_tecla()

        return


    # Verificar se o concurso já começou

    if data_processamento < data_concurso or (data_processamento == data_concurso and hora_processamento < hora_inicio.time()):

        print(f"{Cores.AMARELO}O concurso ainda não começou! A votação do público não pode ser realizada.{Cores.RESET}")

        pressione_tecla()

        return


    # Registar votante

    while True:

        print(f"{Cores.BG_AZUL}[Insira os dados para votar]{Cores.RESET}")

        

        # Identificação do votante

        identificacao = input("Digite a sua identificação (número de telefone): ").strip()

        if not identificacao:

            print(f"{Cores.VERMELHO}A identificação é obrigatória! Tente novamente.{Cores.RESET}")

            continue


        # Idade

        try:

            idade = int(input("Digite a sua idade: ").strip())

            if idade < 16:

                print(f"{Cores.VERMELHO}Pessoas do público devem ter pelo menos 16 anos!{Cores.RESET}")

                continue

        except ValueError:

            print(f"{Cores.VERMELHO}Idade inválida! Digite um número inteiro.{Cores.RESET}")

            continue


        # País de origem do votante

        pais_origem = input("Digite o país do votante: ").strip()

        if not pais_origem or pais_origem not in [p['pais'] for p in participantes]:

            print(f"{Cores.VERMELHO}País inválido! Verifique os participantes.{Cores.RESET}")

            continue

        # Idkey Generator

        IdKey = key()

        # Escolha do país para votar

        while True:

            print(f"Países disponíveis para votar: {[p['pais'] for p in participantes if p['pais'] != pais_origem]}")

            voto = input("Escolha o país que deseja votar: ").strip()

            if voto == pais_origem:

                print(f"{Cores.VERMELHO}Você não pode votar no seu próprio país!{Cores.RESET}")

            elif voto not in [p['pais'] for p in participantes]:

                print(f"{Cores.VERMELHO}País não encontrado entre os participantes!{Cores.RESET}")

            else:

                break


        # Gravar voto no ficheiro de votação

        modo_votos = "a" if os.path.exists(ficheiro_votos) else "w"

        with open(ficheiro_votos, mode=modo_votos, newline='', encoding='utf-8') as ficheiro:

            cabecalho_votos = ["IdKey", "País Origem", "Voto"]

            streamWriter = csv.DictWriter(ficheiro, fieldnames=cabecalho_votos)

            if modo_votos == "w":  # Gravar cabeçalho apenas se o ficheiro for novo

                streamWriter.writeheader()

            streamWriter.writerow({"IdKey": IdKey, "País Origem": pais_origem, "Voto": voto})


        # Gravar eleitor no ficheiro de eleitores

        modo_eleitores = "a" if os.path.exists(ficheiro_eleitores) else "w"

        with open(ficheiro_eleitores, mode=modo_eleitores, newline='', encoding='utf-8') as ficheiro:

            cabecalho_eleitores = ["IdKey","Identificação", "Idade", "País Origem"]

            streamWriter = csv.DictWriter(ficheiro, fieldnames=cabecalho_eleitores)

            if modo_eleitores == "w":  # Gravar cabeçalho apenas se o ficheiro for novo

                streamWriter.writeheader()

            streamWriter.writerow({"IdKey": IdKey ,"Identificação": identificacao, "Idade": idade, "País Origem": pais_origem})


        print(f"{Cores.VERDE}Voto registado com sucesso! Obrigado pela sua participação!{Cores.RESET}")

        

        # Perguntar se deseja continuar

        continuar = input("Deseja continuar a votação? (s/n): ").strip().lower()

        if continuar != "s":

            break


# Registrar um eleitor no arquivo CSV

def registar_eleitor():

    with open('publico.csv', mode='a', newline='', encoding='utf-8') as ficheiro:

        campos = ['Pais', 'ID Eleitor', 'IdKey']
        streamWriter = csv.DictWriter(ficheiro, fieldnames=campos)


        if ficheiro.tell() == 0:

            streamWriter.writeheader()


        while True:

            pais_eleitor = input("Em que país se encontra? ")

            hora_eleitor = input("A que hora está a votar? ")

            id_eleitor = input("Qual é o seu número de telemóvel? (Não pode votar mais de 20 vezes): ")

            key = key()


            streamWriter.writerow({

                'Key': key,

                'Pais': pais_eleitor,

                'ID Eleitor': id_eleitor,              

            })


            print("Você pode proceder à votação agora.")

            votacao_publico()


            continuar = input("Deseja votar novamente? (S/N): ").strip().lower()

            if continuar != 's' or continuar != 'S':

                break


#Função para listar eleitores

def listar_eleitores():


    ficheiro_eleitores = "eleitoresPublico.csv"


    # Verificar se o ficheiro existe

    if not os.path.exists(ficheiro_eleitores):

        print(f"{Cores.AMARELO}Não há eleitores registados! O ficheiro 'eleitoresPublico.csv' não foi encontrado.{Cores.RESET}")
        pressione_tecla()

        return


    print(f"{Cores.BG_AZUL}[--- Lista de Eleitores Registados ---]{Cores.RESET}\n")

    

    # Ler e exibir os dados do ficheiro

    with open(ficheiro_eleitores, mode='r', encoding='utf-8') as ficheiro:

        streamReader = csv.DictReader(ficheiro)

        eleitores = list(streamReader)

        

        if not eleitores:

            print(f"{Cores.AMARELO}O ficheiro está vazio. Nenhum eleitor foi registado.{Cores.RESET}")

        else:

            # Exibir eleitores

            for i, eleitor in enumerate(eleitores, start=1):

                print(f"{Cores.NEGRITO}Eleitor {i}:{Cores.RESET}")
                print(f"  IdKey: {eleitor['IdKey']}")
                print(f"  Identificação: {eleitor['Identificação']}")
                print(f"  Idade: {eleitor['Idade']}")
                print(f"  País Origem: {eleitor['País Origem']}")
                print("")


    print("________________________________")

    pressione_tecla()


#Menu Definições   

def menu_definicoes():

    limpar_ecra()
    definicao = carregar_dados("definicoes")


    while True:


        print(f"{Cores.BG_AZUL}[--- MENU Definições ---]{Cores.RESET}\n")
        print(" 1 - Alterar Definições")
        print(" 2 - Consultar Definições")
        print(" 9 - Voltar ao MENU principal")


        opcao = input("\nEscolha a opção [1, 2 ou 9]: ")

        

        match opcao:

            case "1":

                limpar_ecra()

                alterar_definicoes(definicao)

            case "2":

                limpar_ecra()

                consultar_definicoes(definicao)          

            case "9":

                gravar_dados(definicao, "definicoes")      

                break

            case _:

                print(f"\n{Cores.VERMELHO}Opção inválida! Tente novamente...{Cores.RESET}")

                pressione_tecla()  


#Menu Participantes    

def menu_participantes():

    global participantes
    participantes = carregar_dados("participantes")


    while True:

        limpar_ecra()

        print(f"{Cores.BG_AZUL}[--- MENU Participantes ---]{Cores.RESET}\n")

        print(" 1 - Adicionar participantes")

        print(" 2 - Remover/ Alterar participantes")

        print(" 3 - Consultar lista de participantes")

        print(" 4 - Registar Eleitor")

        print(" 5 - Lista Eleitores")

        print(" 9 - Voltar ao MENU principal")


        opcao = input("\nEscolha a opção [1, 2, 3, 4, 5 ou 9]: ")

        

        match opcao:

            case "1":

                limpar_ecra()
                inserir_participantes(participantes)

            case "2":

                limpar_ecra()
                alterar_remover_participante(participantes)

            case "3":

                limpar_ecra()
                consultar_participante(participantes)

            case "4":
                registar_eleitor()

            case "5":
                limpar_ecra()
                listar_eleitores()

            case "9":

                gravar_dados(participantes, "participantes")      

                break

            case _:

                print(f"\n{Cores.VERMELHO}Opção inválida! Tente novamente...{Cores.RESET}")

                pressione_tecla()  

#Menu Votação

def menu_votacao():

    global participantes
    participantes = carregar_dados("participantes")


    while True:

        limpar_ecra()

        print(f"{Cores.BG_AZUL}[--- MENU Votação ---]{Cores.RESET}\n")
        print(" 1 - Votação do Júri")
        print(" 2 - Votação do Público")
        print(" 3 - Consultar Lista de Eleitores do Público")
        print(" 9 - Voltar ao MENU principal")


        opcao = input("\nEscolha a opção [1, 2 ou 9]: ")

        

        match opcao:

            case "1":

                limpar_ecra()
                votacao_juri()

            case "2":

                limpar_ecra()
                votacao_publico()

            case "3":

                limpar_ecra()          
                listar_eleitores()

            case "9":

                break

            case _:

                print(f"\n{Cores.VERMELHO}Opção inválida! Tente novamente...{Cores.RESET}")
                pressione_tecla()  


#Função MENU Escrutínio

def escrutinio():


    limpar_ecra()

    ficheiro_juri = "votacaoJuri.csv"

    ficheiro_publico = "votacaoPublico.csv"


    resultados_juri = defaultdict(int)

    resultados_publico = defaultdict(int)


    #Ler os votos do júri

    if os.path.exists(ficheiro_juri):

        with open(ficheiro_juri, mode='r', encoding='utf-8') as ficheiro:

            streamReader = csv.DictReader(ficheiro)

            for linha in streamReader:

                pais = linha['País Votado']

                pontos = int(linha['Pontos'])

                resultados_juri[pais] += pontos


    #Ler os votos do público

    if os.path.exists(ficheiro_publico):

        with open(ficheiro_publico, mode='r', encoding='utf-8') as ficheiro:

            streamReader = csv.DictReader(ficheiro)

            for linha in streamReader:

                pais = linha['Voto']

                resultados_publico[pais] += 1  # Cada voto do público vale 1 ponto


    #Combinar resultados

    resultados_totais = defaultdict(int)

    for pais in set(resultados_juri.keys()).union(resultados_publico.keys()):

        resultados_totais[pais] = resultados_juri[pais] + resultados_publico[pais]


    #Ordenar os resultados por pontuação em ordem decrescente

    resultados_juri_ordenados = sorted(resultados_juri.items(), key=lambda item: item[1], reverse=True)

    resultados_publico_ordenados = sorted(resultados_publico.items(), key=lambda item: item[1], reverse=True)

    resultados_totais_ordenados = sorted(resultados_totais.items(), key=lambda item: item[1], reverse=True)


    #Exibir os resultados do júri

    print(f"{Cores.BG_AZUL}[--- RESULTADOS DO JÚRI ---]{Cores.RESET}\n")

    for i, (pais, pontos) in enumerate(resultados_juri_ordenados, start=1):

        print(f"{Cores.BG_VERDE}{i}º lugar: {pais} com {pontos} pontos{Cores.RESET}")


    print("________________________________\n")


    #Exibir os resultados do público

    print(f"{Cores.BG_AZUL}[--- RESULTADOS DO PÚBLICO ---]{Cores.RESET}\n")

    for i, (pais, votos) in enumerate(resultados_publico_ordenados, start=1):

        print(f"{Cores.BG_AMARELO}{i}º lugar: {pais} com {votos} votos{Cores.RESET}")


    print("________________________________\n")


    #Exibir os resultados totais

    print(f"{Cores.BG_AZUL}[--- RESULTADOS TOTAIS ---]{Cores.RESET}\n")

    for i, (pais, total) in enumerate(resultados_totais_ordenados, start=1):
        print(f"{Cores.BG_CIANO}{i}º lugar: {pais} com {total} pontos totais{Cores.RESET}")


    print("________________________________")

    pressione_tecla()




#Adicionar participantes

def inserir_participantes(participantes):

    

    limpar_ecra()

    definicao=carregar_dados("definicoes")
    limite=int(definicao['Numero Maximo de Participantes'])
    atuais=len(participantes)


    if len(participantes) == limite:

        print(f"O limite de {limite} participante já foi atingido!")
        input(f"{Cores.AMARELO}Prima qualquer tecla para voltar atrás.{Cores.RESET}")

        return

    else: 

        print(f"{Cores.BG_AZUL}Faltam inserir {(limite-atuais)} participante(s).")  


    print(f"Se pressionar ENTER no nome do Participante vair PARAR de inserir{Cores.RESET}\n")


    #colocar o número certo de participantes

    while len(participantes) < (limite):

        nome = input("Digite o nome do participante: ")

        if not nome:

            limpar_ecra()

            break


        while True:

            try:       

                idade = int(input("Digite a idade do participante: ").strip())       

                if idade < 16:

                    print("Não pode concorrer no Festival da Eurovisão! Idade mínima é 16 anos.")

                    continue  

                elif idade <= 0:

                    print("Idade inválida! Por favor, insira uma idade maior que 0.")

                    continue  

                else:              

                    break  

            except ValueError:

                print("Entrada inválida! Por favor, insira um número inteiro válido.")


        pais = input("Digite o país do participante: ")
        musica = input("Qual o nome da música? ")

        

        while True:

            try:

                tempo_musica = float(input('Digite quanto tempo tem a música (em minutos): ').strip())

                if tempo_musica > 3:

                    print("A música tem tempo a mais. Não pode concorrer ao Festival da Eurovisão!")

                    continue  

                elif tempo_musica <= 0:

                    print("O tempo da música deve ser maior que 0. Tente novamente!")

                    continue  

                else:

                    break  

            except ValueError:

                print("Entrada inválida! Por favor, insira um número decimal válido.")


        participante = {

            'IdKey': key(),
            'nome': nome,
            'idade': idade,
            'pais': pais,
            'musica': musica,
            'tempo_musica': tempo_musica

        }


        participantes.append(participante)
        gravar_dados(participantes, "participantes")
        print(f"{Cores.VERDE}Participante adicionado com sucesso!{Cores.RESET}")


#Remover/Alterar participantes

def alterar_remover_participante(participantes):

    if not participantes:

        print(f"{Cores.BG_VERMELHO}Não há participantes para alterar ou remover.{Cores.RESET}")

        pressione_tecla()

        return


    listar_participantes(participantes)

    nome = input("Digite o nome do participante que quer alterar ou remover: ")
    participante = next((p for p in participantes if p['nome'].lower() == nome.lower()), None)


    if not participante:

        print(f"{Cores.VERMELHO}Participante não encontrado.{Cores.RESET}")

        return


    modo = input("Digite 'alterar' para modificar ou 'remover' para retirar o participante: ").lower()


    if modo == 'remover':

        participantes.remove(participante)

        print("\033[33;42mParticipante removido com sucesso.\033[0m")

    elif modo == 'alterar':

        try:

            idade = int(input("Digite a nova idade do participante: "))

            if idade < 15:

                print("Não pode concorrer no Festival da Eurovisão! Idade mínima é 15 anos.")

                return


            pais = input("Digite o novo país do participante: ")
            musica = input("Qual o novo nome da música? ")
            tempo_musica = float(input('Digite o novo tempo da música (em minutos): '))


            if tempo_musica > 3:

                print("A música tem tempo a mais. Não pode concorrer ao Festival da Eurovisão!")

                return


            participante.update({

                'idade': idade,
                'pais': pais,
                'musica': musica,
                'tempo_musica': tempo_musica

            })

            gravar_dados(participantes, "participantes") 

            print(f"{Cores.VERDE}Participante alterado com sucesso!{Cores.RESET}")

            input(f"{Cores.AMARELO}Prima qualquer tecla para voltar atrás.{Cores.RESET}")


        except ValueError:

            print("Não foi possivel alterar ou remover o participante. Por favor tente novamente.")
            input(f"{Cores.AMARELO}Prima qualquer tecla para voltar atrás.{Cores.RESET}")

    else:

        print("Ação inválida.")
        input(f"{Cores.AMARELO}Prima qualquer tecla para voltar atrás.{Cores.RESET}")


# 3 - Consultar lista de participantes

def listar_participantes(participantes):

    if not participantes:

        print(f"{Cores.BG_AMARELO}Não há nenhum participante registado.{Cores.RESET}")

    else:

        print(f"\n{Cores.BG_AZUL}[Lista de participantes]\n{Cores.RESET}")

        for participante in participantes:

            print(f"Nome: {participante['nome']}, Idade: {participante['idade']}, País: {participante['pais']}, Música: {participante['musica']}, Tempo: {participante['tempo_musica']} min")



def consultar_participante(participantes):

    limpar_ecra()
    listar_participantes(participantes)    
    input(f"\n{Cores.AMARELO}[qq tecla para voltar atrás]{Cores.RESET}")
    limpar_ecra()


# Função para verificar e criar o ficheiro definicoes.csv

def verificar_ou_criar_definicoes():

    global definicao

    if not os.path.exists("definicoes.csv"):

        print("\n\033[33;43m[Sistema ainda não configurado...]\033[0m\n")

        

        numero_max_participantes = input("Digite o Número Máximo de Participantes: ")

        

        while True:

            data_concurso_out = input("Digite a Data do Concurso (formato DD-MM-AAAA): ")
            data_concurso = validar_data(data_concurso_out)

            if data_concurso:

                break

        

        while True:

            hora_inicio_out = input("Digite a Hora de Início do Concurso (formato HH:MM): ")
            hora_inicio = validar_hora(hora_inicio_out)

            if hora_inicio:

                break

        

        while True:

            hora_fim_out = input("Digite a Hora de Fim do Concurso (formato HH:MM): ")
            hora_fim = validar_hora(hora_fim_out)

            if hora_fim:

                if hora_inicio < hora_fim:

                    break

                else:

                    print("Hora de fim deve ser posterior à hora de início.")

        

        nome_concurso = "Festival da Eurovisão da Canção"

        

        # Criar o arquivo definicoes.csv

        definicao = {

            "Numero Maximo de Participantes": int(numero_max_participantes),
            "Nome do Concurso": nome_concurso,
            "Data do Concurso": data_concurso,
            "Hora Inicio": hora_inicio,
            "Hora Fim": hora_fim,

        }       

        #definicoes.append(definicao)

        gravar_dados(definicao,"definicoes")
        print(f"\n{Cores.VERDE}[Configurações gravadas com sucesso!]{Cores.RESET}\n")

    else:

        definicao = carregar_dados("definicoes")
        print(f"\n{Cores.AZUL}[Configuração do sistema já efetuada e válida!]{Cores.RESET}\n")


# Função para listar os votos do júri e do público.

def listar_votos():

    

    ficheiro_juri = "votacaoJuri.csv"
    ficheiro_publico = "votacaoPublico.csv"


    print(f"{Cores.BG_AZUL}[--- Lista de Votos ---]{Cores.RESET}\n")

    

    # Listar votos do júri
    print(f"{Cores.BG_AZUL}[Votos do Júri]{Cores.RESET}\n")

    if not os.path.exists(ficheiro_juri):

        print(f"{Cores.AMARELO}Não há votos registados do júri!{Cores.RESET}")

    else:

        with open(ficheiro_juri, mode='r', encoding='utf-8') as ficheiro:

            streamReader = csv.DictReader(ficheiro)
            votos_juri = list(streamReader)

            

            if not votos_juri:

                print(f"{Cores.AMARELO}O ficheiro está vazio. Nenhum voto do júri foi registado.{Cores.RESET}")

            else:

                for i, voto in enumerate(votos_juri, start=1):

                    print(f"{Cores.NEGRITO}Voto {i}:{Cores.RESET}")
                    print(f"  Jurado: {voto['IdKey']}")
                    print(f"  País Votado: {voto['País Votado']}")
                    print(f"  Pontos: {voto['Pontos']}")
                    print("")

    

    print("________________________________\n")

    

    # Listar votos do público
    print(f"{Cores.BG_AZUL}[Votos do Público]{Cores.RESET}\n")

    if not os.path.exists(ficheiro_publico):

        print(f"{Cores.AMARELO}Não há votos registados do público!{Cores.RESET}")

    else:

        with open(ficheiro_publico, mode='r', encoding='utf-8') as ficheiro:

            streamReader = csv.DictReader(ficheiro)
            votos_publico = list(streamReader)

            

            if not votos_publico:

                print(f"{Cores.AMARELO}O ficheiro está vazio. Nenhum voto do público foi registado.{Cores.RESET}")

            else:

                for i, voto in enumerate(votos_publico, start=1):

                    print(f"{Cores.NEGRITO}Voto {i}:{Cores.RESET}")
                    print(f"  Eleitor: {voto['IdKey']}")
                    print(f"  País Origem: {voto['País Origem']}")
                    print(f"  Voto: {voto['Voto']}")
                    print("")

    

    print("________________________________")
    pressione_tecla()


def menu_escrutinio():

    while True:

        limpar_ecra()

        print(f"{Cores.BG_AZUL}[--- MENU Escrutínio ---]{Cores.RESET}\n")
        print(" 1 - Resultados Finais")
        print(" 2 - Listar Votos do Júri e do Público")
        print(" 9 - Voltar ao MENU principal")


        opcao = input("\nEscolha a opção [1, 2 ou 9]: ")

        

        match opcao:

            case "1":

                limpar_ecra()
                escrutinio()

            case "2":

                limpar_ecra()
                listar_votos()

            case "9":

                break

            case _:

                print(f"\n{Cores.VERMELHO}Opção inválida! Tente novamente...{Cores.RESET}")
                pressione_tecla()


def menu_principal():

    global data_processamento
    global hora_processamento
    global definicao

    print(f"{Cores.NEGRITO}Bem-Vindo ao programa de votação para o Festival da Eurovisão!{Cores.RESET}\n")
    print(f"{Cores.BG_AZUL}[--- MENU PRINCIPAL ---]\n")
    print(f"Data/Hora de processamento: {data_processamento} / {hora_processamento}{Cores.RESET}\n")
    print("1. Definições")
    print("2. Países participantes")
    print("3. Votação")
    print("4. Escrutínio")
    print("9. Sair")


# Função Main

def inicio():


    global data_processamento
    global hora_processamento
    global data_concurso
    global hora_inicio
    global hora_fim
    global definicao
    global votos

    votos = defaultdict(int)

    

    while True:

        data_out=input("Qual a data de processamento (Formato: DD-MM-AAAA)? ")
        data_processamento = validar_data(data_out)

        if data_processamento:

            break

    while True:

        hora_out=input("Qual a hora de processamento (Formato: HH:MM)? ")
        hora_processamento = validar_hora(hora_out)

        if hora_processamento:

            break

          

    data_concurso=datetime.strptime(definicao['Data do Concurso'], "%d-%m-%Y") 

    hora_inicio=datetime.strptime(definicao['Hora Inicio'], "%H:%M")

    hora_fim=datetime.strptime(definicao['Hora Fim'],"%H:%M")


    while True:

        limpar_ecra()

        menu_principal()


        opcao = input("\nEscolha a opção [1, 2, 3, 4 ou 9]: ")


        match opcao:

            case "1":

                menu_definicoes()

            case "2":

                if data_processamento > data_concurso:

                    print(f"\nConcurso já terminou, só poderá proceder á votação.")
                    pressione_tecla()

                elif data_processamento == data_concurso and hora_processamento > hora_inicio.time():

                    print(f"\nConcurso já começou, inscrição de participantes está fechada.")
                    pressione_tecla()

                else:

                    menu_participantes()

            case "3":

                if data_processamento < data_concurso:

                    print(f"\nAinda não começou o concurso")
                    pressione_tecla()

                else:

                    menu_votacao()

            case "4":      

                if  data_processamento < data_concurso:

                    print(f"\nAinda não começou o concurso")
                    pressione_tecla()

                elif data_processamento == data_concurso and hora_processamento < hora_fim.time() :

                    print(f"\nAinda não terminou o concurso")
                    pressione_tecla()             

                else:

                    menu_escrutinio()

            case "9":

                print(f"\n{Cores.AMARELO}[Obrigado por usar o Portal! Até breve.]{Cores.RESET}\n")
                break

            case _:

                print(f"\n{Cores.VERMELHO}Opção inválida! Tente novamente...{Cores.RESET}")
                pressione_tecla()  




# Iniciar o programa
if __name__ == "__main__":

    limpar_ecra()
    verificar_ou_criar_definicoes()
    inicio()