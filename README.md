# Piet-Mondrian-
import tkinter as tk
from random import randint, shuffle

class MondrianDesignGenerator:
    def __init__(self, root):
        self.root = root
        self.root.title("Mondrian Design Generator")

        self.canvas = tk.Canvas(root, width=500, height=500, bg='#ffffff')
        self.canvas.pack(expand=tk.YES, fill=tk.BOTH)

        self.generate_button = tk.Button(root, text="Generar Diseño", command=self.generate_design)
        self.generate_button.pack(pady=10)

        self.generate_design()

    def check_overlap(self, x1, y1, x2, y2, rectangles):
        for rect in rectangles:
            if (x1 < rect[2] and x2 > rect[0] and y1 < rect[3] and y2 > rect[1]):
                return True  #solapamiento
        return False

    def check_within_lines(self, x1, y1, x2, y2, lines):
        for line in lines:
            if (x1 < line[2] and x2 > line[0] and y1 < line[3] and y2 > line[1]):
                return False  # El rectángulo sobre la línea
        return True

    def generate_design(self):
        self.canvas.delete("all")  # Limpiar fondo

        # Colores para los rectángulos y cuadrados
        colors = ["#de0404", "#f9f745", "#eff0ee", "#ceccd2", "#1b216c"]

        shuffle(colors)  # Mezclar colores

        rectangles = []  # Almacenar coordenadas de rectángulos 
        lines = []  # Almacenar coordenadas de líneas 

        #líneas horizontales y verticales
        for i in range(randint(4, 7)):
            y = randint(50, 450)
            self.canvas.create_line(50, y, 450, y, fill='#0b0a1a', width=randint(2, 4))
            lines.append((50, y, 450, y))

        for i in range(randint(4, 7)):
            x = randint(50, 450)
            self.canvas.create_line(x, 50, x, 450, fill='#0b0a1a', width=randint(2, 4))
            lines.append((x, 50, x, 450))

        #rectángulos y cuadrados 
        for i in range(randint(3, 7)):
            while True:
                x1 = randint(50, 400)
                y1 = randint(50, 400)
                width = randint(50, 150)
                height = randint(50, 150)
                x2 = x1 + width
                y2 = y1 + height

                if not self.check_overlap(x1, y1, x2, y2, rectangles) and self.check_within_lines(x1, y1, x2, y2, lines):
                    break

            color = colors[i % len(colors)]  # Cambiar colores
            self.canvas.create_rectangle(x1, y1, x2, y2, fill=color, outline='#0b0a1a', width=randint(5, 7))

            rectangles.append((x1, y1, x2, y2))

if __name__ == "__main__":
    root = tk.Tk()
    app = MondrianDesignGenerator(root)
    root.mainloop()

