import time


usuarios = []
contas = []

class Usuario:
    def __init__(self, nome, cpf):
        self.nome = nome
        self.cpf = cpf

class ContaCorrente:
    def __init__(self, usuario):
        self.usuario = usuario
        self.deposito = 0
        self.saque = 0
        self.extrato_deposito = []
        self.limite_saque_diario = 3
        self.conte_saque_diario = 0
        self.limite_para_saque = 500
        self.extrato_saque = []
        self.extrato = []
        self.saldo_deposito = 0
        self.saldo_sacar = 0

def menu():
    print(''' 
    MENU
          
[1] Deposito
[2] Saque
[3] Extrato
[4] Criar Usuário
[5] Criar Conta Corrente
[6] Sair''')

def depositar(conta):
    depositar = float(input('Quanto deseja depositar? R$ '))

    if depositar > 0:
        print('Depositando...')
        time.sleep(2)
        print(f'Deposito Realizado! Valor do deposito R$ {depositar:.2f}')
        conta.extrato_deposito.append(depositar)
        conta.extrato.append(f'Deposito: R$ {depositar:.2f} ')
        conta.saldo_deposito += depositar
    else:
        print('Nenhum deposito foi realizado!')

def sacar(conta):
    if conta.conte_saque_diario >= conta.limite_saque_diario:
        print('Limite de saque diário atingido!')
        return

    sacar = float(input('Quanto deseja sacar? R$ '))

    if sacar <= conta.saldo_deposito:
        if sacar > conta.limite_para_saque:
            print('Limite máximo é de R$ 500.')
        elif sacar <= 0:
            print('Não foi possivel realizar o saque!')
        else:
            print('Realizando o saque....')
            time.sleep(2)
            print(f'Saque realizado! Valor de R$ {sacar:.2f}')
            conta.extrato_saque.append(sacar)
            conta.extrato.append(f'Saque: R$ {sacar:.2f}')
            conta.saldo_sacar += sacar
            conta.saldo_deposito -= sacar
            conta.conte_saque_diario += 1
    else:
        print('Saldo insuficiente.') 

def visualizar_historico(conta):
    print('='*15, 'EXTRATO' ,'='*15)
    if not conta.extrato:
        print('Nenhuma movimentação realizada!')
    else:
        for item in conta.extrato:
            print(item)
    saldo_total = conta.saldo_deposito - conta.saldo_sacar
    print(f'Saldo: R$ {saldo_total:.2f}')
    print('='*39)

def criar_usuario():
    nome = input('Digite o nome do usuário: ')
    cpf = input('Digite o CPF do usuário: ')
    usuario = Usuario(nome, cpf)
    usuarios.append(usuario)
    print('Usuário criado com sucesso!')

def criar_conta_corrente():
    cpf = input('Digite o CPF do usuário para vincular a conta: ')
    usuario = next((usuario for usuario in usuarios if usuario.cpf == cpf), None)

    if usuario:
        conta = ContaCorrente(usuario)
        contas.append(conta)
        print('Conta corrente criada com sucesso!')
    else:
        print('Usuário não encontrado!')


while True:
    menu()
    opção = int(input('Escolha uma opção: '))

    if opção == 1:
        if contas:
            conta_atual = contas[0]  
            depositar(conta_atual)
        else:
            print('Nenhuma conta encontrada!')

    elif opção == 2:
        if contas:
            conta_atual = contas[0]  
            sacar(conta_atual)
        else:
            print('Nenhuma conta encontrada!')

    elif opção == 3:
        if contas:
            conta_atual = contas[0]  
            visualizar_historico(conta_atual)
        else:
            print('Nenhuma conta encontrada!')

    elif opção == 4:
        criar_usuario()

    elif opção == 5:
        criar_conta_corrente()

    elif opção == 6:
        print('Saindo... Obrigado!!')
        break
    else:
        print('Opção inválida!')

