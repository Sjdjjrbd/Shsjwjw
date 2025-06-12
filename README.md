class Personagem:
    def __init__(self, nome):
        self.nome = nome
        self.altura = 0
        self.voando = False

    def voar(self):
        if not self.voando:
            print(f"{self.nome} começou a voar!")
            self.voando = True
        else:
            print(f"{self.nome} já está voando.")

    def subir(self, metros):
        if self.voando:
            self.altura += metros
            print(f"{self.nome} subiu para {self.altura} metros de altura.")
        else:
            print(f"{self.nome} precisa começar a voar primeiro!")

    def descer(self, metros):
        if self.voando and self.altura > 0:
            self.altura = max(0, self.altura - metros)
            print(f"{self.nome} desceu para {self.altura} metros de altura.")
            if self.altura == 0:
                self.pousar()
        else:
            print(f"{self.nome} já está no chão.")

    def pousar(self):
        if self.voando:
            print(f"{self.nome} pousou.")
            self.voando = False
            self.altura = 0
        else:
            print(f"{self.nome} já está no chão.")

# Exemplo de uso:
heroi = Personagem("Super Herói")
heroi.voar()
heroi.subir(10)
heroi.subir(5)
heroi.descer(8)
heroi.descer(7)
