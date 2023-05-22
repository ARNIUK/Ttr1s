# Ttr1simport pygame
import random

# Inicializar pygame
pygame.init()

# Definir colores
NEGRO = (0, 0, 0)
BLANCO = (255, 255, 255)
AZUL = (0, 0, 255)
VERDE = (0, 255, 0)
ROJO = (255, 0, 0)

# Definir dimensiones de la ventana
ANCHO_VENTANA = 800
ALTO_VENTANA = 600

# Definir dimensiones de los bloques
ANCHO_BLOQUE = 30
ALTO_BLOQUE = 30

# Definir velocidad de caída de los bloques
VELOCIDAD_CAIDA = 10

# Definir clase para representar los bloques
class Bloque(pygame.sprite.Sprite):
    def __init__(self, color):
        super().__init__()
        self.image = pygame.Surface([ANCHO_BLOQUE, ALTO_BLOQUE])
        self.image.fill(color)
        self.rect = self.image.get_rect()

    def update(self):
        self.rect.y += VELOCIDAD_CAIDA

# Crear ventana
ventana = pygame.display.set_mode([ANCHO_VENTANA, ALTO_VENTANA])
pygame.display.set_caption("Tetris")

# Crear lista de todos los sprites
todos_los_sprites = pygame.sprite.Group()

# Crear lista de bloques en movimiento
bloques_en_movimiento = pygame.sprite.Group()

# Crear bloque inicial
bloque = Bloque(AZUL)
bloque.rect.x = 365
bloque.rect.y = 55

# Agregar el bloque a las listas de sprites
todos_los_sprites.add(bloque)
bloques_en_movimiento.add(bloque)

# Variable para controlar el ciclo del juego
jugando = True

# Reloj para controlar la velocidad de actualización de la pantalla
reloj = pygame.time.Clock()

# Bucle principal del juego
while jugando:
    # Manejo de eventos
    for evento in pygame.event.get():
        if evento.type == pygame.QUIT:
            jugando = False

    # Generar nuevos bloques aleatoriamente
    if len(bloques_en_movimiento) < 10:
        for i in range(5):
            bloque = Bloque(VERDE)
            bloque.rect.x = random.randrange(ANCHO_VENTANA - ANCHO_BLOQUE)
            bloque.rect.y = random.randrange(-ALTO_BLOQUE, -ALTO_BLOQUE * 10)
            bloques_en_movimiento.add(bloque)
            todos_los_sprites.add(bloque)

    # Actualizar posición de los bloques
    bloques_en_movimiento.update()

    # Limpiar pantalla
    ventana.fill(NEGRO)

    # Dibujar todos los sprites
    todos_los_sprites.draw(ventana)

    # Actualizar pantalla
    pygame.display.flip()

    # Limitar la velocidad de actualización
    reloj.tick(60)

# Cerrar pygame
pygame.quit()
