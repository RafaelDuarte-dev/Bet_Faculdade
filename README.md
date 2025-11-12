# Bet_Faculdade
[main.py](https://github.com/user-attachments/files/23504693/main.py)
[Aposta.py](https://github.com/user-attachments/files/23504825/Aposta.py)
import random


class Aposta:
    def __init__(self, jogador, time1, time2, valor):
        self.jogador = jogador
        self.time1 = time1
        self.time2 = time2
        self.valor = valor

        # Simula 10 partidas entre os times
        self.historico = self.simular_partidas()

        # Calcula as odds com base nos resultados
        self.odds = self.calcular_odds()

    def simular_partidas(self):
        """Simula 10 partidas e retorna o número de vitórias de cada time."""
        vitorias = {self.time1: 0, self.time2: 0}
        for _ in range(10):
            vencedor = random.choice([self.time1, self.time2])
            vitorias[vencedor] += 1
        return vitorias

    def calcular_odds(self):
        """Calcula odds baseadas nas vitórias simuladas."""
        total = sum(self.historico.values())
        prob1 = self.historico[self.time1] / total
        prob2 = self.historico[self.time2] / total

        # Odds inversas das probabilidades, com limite mínimo/máximo
        odd1 = round(max(1.1, min(3.0, 1 / prob1)), 2)
        odd2 = round(max(1.1, min(3.0, 1 / prob2)), 2)

        return {self.time1: odd1, self.time2: odd2}

    def mostrar_odds(self):
        print(f"\nOdds baseadas em 10 partidas entre os times:")
        print(f"{self.time1} venceu {self.historico[self.time1]} vezes → Odd: {self.odds[self.time1]}")
        print(f"{self.time2} venceu {self.historico[self.time2]} vezes → Odd: {self.odds[self.time2]}")

    def play(self, time_apostado):
        """Executa a aposta com base no time escolhido e odds calculadas."""
        print(f"\nA partida será entre {self.time1} x {self.time2}!")
        print("Rodando o resultado...\n")

        vencedor = random.choice([self.time1, self.time2])
        print(f"O time vencedor foi: {vencedor}!")

        if vencedor == time_apostado:
            ganho = self.valor * self.odds[time_apostado]
            self.jogador.ganhar(ganho)
            print(f"Parabéns, {self.jogador.nome}! Você ganhou R$ {ganho:.2f}")
        else:
            self.jogador.perder(self.valor)
            print(f"Que pena, {self.jogador.nome}! Você perdeu R$ {self.valor:.2f}")

        print(f"Saldo atual: R$ {self.jogador.saldo:.2f}\n")

[Apostador.py](https://github.com/user-attachments/files/23504832/Apostador.py)
class Apostador:
    def __init__(self, nome, idade, saldo_inicial=100):
        self.nome = nome
        self.idade = idade
        self.saldo = saldo_inicial

    def apostar(self, valor):
        if valor > self.saldo:
            print(f"Saldo insuficiente! Seu saldo atual é R$ {self.saldo:.2f}")
            return False
        self.saldo -= valor
        return True

    def ganhar(self, valor):
        """Adiciona valor ao saldo quando o jogador ganha."""
        self.saldo += valor

    def perder(self, valor):
        """Remove valor do saldo quando o jogador perde."""
        self.saldo -= valor






