# TPC4---Aplica-o-para-gerir-um-cinema
def menu():
    cinema = []
    while True:
        print("Menu:")
        print("1 - Inserir nova sala")
        print("2 - Filmes em exibição")
        print("3 - Verificar disponibilidade de lugar")
        print("4 - Vender bilhete(s)")
        print("5 - Verificar disponibilidades")
        print("6 - Remover sala")
        print("7 - Cancelar bilhete")
        print("8 - Mudar filme de uma sala")
        print("9 - Ver detalhes de uma sala")
        print("0 - Sair")
        opc = input("Escolha uma opção: ")
        if opc == "1":
            num_lug = ler_int("Número de lugares da sala: ")
            filme = input("Nome do filme: ")
            sala = [num_lug, [], filme]
            cinema = inserirSala(cinema, sala)
        elif opc == "2":
            listar(cinema)
        elif opc == "3":
            filme = input("Nome do filme: ")
            lugar = ler_int("Número do lugar: ")
            if disponivel(cinema, filme, lugar):
                print("Disponível.")
            else:
                print("Não disponível.")
        elif opc == "4":
            filme = input("Nome do filme: ")
            lugar = ler_int("Número do lugar: ")
            cinema = vendebilhete(cinema, filme, lugar)
        elif opc == "5":
            listardisponibilidades(cinema)
        elif opc == "6":
            filme = input("Nome do filme a remover: ")
            cinema = removerSala(cinema, filme)
        elif opc == "7":
            filme = input("Nome do filme: ")
            lugar = ler_int("Número do lugar a cancelar: ")
            cinema = cancelarBilhete(cinema, filme, lugar)
        elif opc == "8":
            filme_ant = input("Filme atual na sala: ")
            filme_novo = input("Novo filme: ")
            cinema = mudarFilme(cinema, filme_ant, filme_novo)
        elif opc == "9":
            filme = input("Nome do filme: ")
            detalhesSala(cinema, filme)
        elif opc == "0":
            print("Programa Encerrado.")
            break
        else:
            print("Opção inválida. Tente novamente.")

def inserirSala(cinema, sala):
    for i in cinema:
        if i[2] == sala[2]:
            print(f"Sala com o filme '{sala[2]}' já existe.")
            return cinema
    cinema.append(sala)
    print(f"Sala com o filme '{sala[2]}' adicionada.")
    return cinema

def listar(cinema):
    if not cinema:
        print("Nenhuma sala encontrada.")
        return
    print("Filmes em exibição: ")
    for i in cinema:
        print(f" - {i[2]}")

def disponivel(cinema, filme, lugar):
    for i in cinema:
        if i[2] == filme:
            if lugar < 1 or lugar > i[0]:
                return False
            return lugar not in i[1]
    return False

def vendebilhete(cinema, filme, lugar):
    for i in cinema:
        if i[2] == filme:
            if lugar < 1 or lugar > i[0]:
                print("Número de lugar inválido.")
            elif lugar in i[1]:
                print(f"Lugar {lugar} já vendido para '{filme}'.")
            else:
                i[1].append(lugar)
                print(f"Bilhete vendido para '{filme}', lugar {lugar}.")
            return cinema
    print(f"Filme '{filme}' não encontrado. A venda não pode ser realizada.")
    return cinema

def listardisponibilidades(cinema):
    if not cinema:
        print("Nenhuma sala encontrada.")
        return
    print("Disponibilidades por sala:")
    for i in cinema:
        capacidade = i[0]
        vendidos = len(i[1])
        disponiveis = capacidade - vendidos
        print(f" - '{i[2]}': {disponiveis} lugares disponíveis (capacidade {capacidade}).")

def removerSala(cinema, filme):
    for i in cinema:
        if i[2] == filme:
            cinema.remove(i)
            print(f"Sala com o filme '{filme}' removida.")
            return cinema
    print(f"Filme '{filme}' não encontrado.")
    return cinema

def cancelarBilhete(cinema, filme, lugar):
    for i in cinema:
        if i[2] == filme:
            if lugar in i[1]:
                i[1].remove(lugar)
                print(f"Bilhete cancelado: '{filme}', lugar {lugar}.")
            else:
                print(f"Lugar {lugar} não está à venda para '{filme}'.")
            return cinema
    print(f"Filme '{filme}' não encontrado. Nenhum cancelamento efetuado.")
    return cinema

def mudarFilme(cinema, filme_antigo, filme_novo):
    for i in cinema:
        if i[2] == filme_novo:
            print(f"Já existe uma sala com o filme '{filme_novo}'. Não é possível efetuar a mudança.")
            return cinema
    for i in cinema:
        if i[2] == filme_antigo:
            i[2] = filme_novo
            print(f"Filme na sala alterado de '{filme_antigo}' para '{filme_novo}'.")
            return cinema
    print(f"Filme '{filme_antigo}' não encontrado. Mudança não alterada.")
    return cinema

def detalhesSala(cinema, filme):
    for i in cinema:
        if i[2] == filme:
            print(f"Detalhes da sala - Filme: '{filme}'")
            print(f" Capacidade: {i[0]}")
            print(f" Lugares vendidos (total {len(i[1])}): {sorted(i[1])}")
            return
    print(f"Filme '{filme}' não encontrado.")

def ler_int(x):
    while True:
        try:
            return int(input(x))
        except ValueError:
            print("Por favor introduza um número inteiro válido.")

if __name__ == "__main__":
    menu()
