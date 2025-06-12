class Character:
def __init__(self, name):
self.name = name
self.height = 0
self.flying = False

def fly(self):
if not self.flying:
print(f"{self.name} has started flying!")
self.flying = True
else:
print(f"{self.name} is already flying.")

def climb(self, meters):
if self.flying:
self.height += meters
print(f"{self.name} has climbed to {self.height} meters high.")
else:
print(f"{self.name} needs to start flying first!")

def descend(self, meters):
if self.flying and self.height > 0:
self.height = max(0, self.height - meters)
print(f"{self.name} has descended to {self.height} meters high.")
if self.height == 0:
self.landing()
else:
print(f"{self.name} is already on the ground.")

def landing(self):
if self.flying:
print(f"{self.name} has landed.")
self.flying = False
self.height = 0
else:
print(f"{self.name} is already on the ground.")

# Usage example:
hero = Character("Super Hero")
hero.fly()
hero.climb(10)
hero.climb(5)
hero.climb(8)
hero.climb(7) 
