# Práctica: Medición de tiempo de ejecución con flujos con y sin buffer

## 1. Código Fuente
# Práctica: Medición de tiempo de ejecución con flujos con y sin buffer

## 1. Código Fuente

```java
import java.io.*;

public class EscrituraBuffered {

    public static void main(String[] args) {
        int n = 10_000_000; // Cambia este valor según la prueba (100_000, 1_000_000, 10_000_000)
        String rutaTexto   = "archivo_salida.txt";
        String rutaBinario = "archivo_salida.dat";

        String modo = args.length > 0 ? args[0] : "todos";

        System.out.println("=== Modo: " + modo + " | n = " + n + " ===");

        switch (modo) {
            case "sinBuffer":
                escribirFileWriter(rutaTexto, n);
                leerFileReader(rutaTexto);
                escribirFileOutputStream(rutaBinario, n);
                leerFileInputStream(rutaBinario);
                break;
            case "conBuffer":
                escribirBufferedWriter(rutaTexto, n);
                leerBufferedReader(rutaTexto);
                escribirBufferedOutputStream(rutaBinario, n);
                leerBufferedInputStream(rutaBinario);
                break;
            default:
                escribirFileWriter(rutaTexto, n);
                escribirBufferedWriter(rutaTexto, n);
                leerFileReader(rutaTexto);
                leerBufferedReader(rutaTexto);
                escribirFileOutputStream(rutaBinario, n);
                escribirBufferedOutputStream(rutaBinario, n);
                leerFileInputStream(rutaBinario);
                leerBufferedInputStream(rutaBinario);
        }
    }

    // ---------- Texto: escritura ----------
    private static void escribirFileWriter(String ruta, int n) {
        long inicio = System.nanoTime();
        try (FileWriter fw = new FileWriter(ruta)) {
            for (int i = 0; i < n; i++) fw.write("Linea " + i + "\n");
        } catch (IOException e) { e.printStackTrace(); }
        long fin = System.nanoTime();
        System.out.printf("FileWriter          escritura: %8.3f ms%n", (fin - inicio) / 1_000_000.0);
    }

    private static void escribirBufferedWriter(String ruta, int n) {
        long inicio = System.nanoTime();
        try (BufferedWriter bw = new BufferedWriter(new FileWriter(ruta))) {
            for (int i = 0; i < n; i++) bw.write("Linea " + i + "\n");
        } catch (IOException e) { e.printStackTrace(); }
        long fin = System.nanoTime();
        System.out.printf("BufferedWriter      escritura: %8.3f ms%n", (fin - inicio) / 1_000_000.0);
    }

    // ---------- Texto: lectura ----------
    private static void leerFileReader(String ruta) {
        long inicio = System.nanoTime();
        try (FileReader fr = new FileReader(ruta)) {
            while (fr.read() != -1) { }
        } catch (IOException e) { e.printStackTrace(); }
        long fin = System.nanoTime();
        System.out.printf("FileReader          lectura  : %8.3f ms%n", (fin - inicio) / 1_000_000.0);
    }

    private static void leerBufferedReader(String ruta) {
        long inicio = System.nanoTime();
        try (BufferedReader br = new BufferedReader(new FileReader(ruta))) {
            while (br.readLine() != null) { }
        } catch (IOException e) { e.printStackTrace(); }
        long fin = System.nanoTime();
        System.out.printf("BufferedReader      lectura  : %8.3f ms%n", (fin - inicio) / 1_000_000.0);
    }

    // ---------- Binario: escritura ----------
    private static void escribirFileOutputStream(String ruta, int n) {
        long inicio = System.nanoTime();
        try (FileOutputStream fos = new FileOutputStream(ruta)) {
            for (int i = 0; i < n; i++) fos.write((i + "\n").getBytes());
        } catch (IOException e) { e.printStackTrace(); }
        long fin = System.nanoTime();
        System.out.printf("FileOutputStream    escritura: %8.3f ms%n", (fin - inicio) / 1_000_000.0);
    }

    private static void escribirBufferedOutputStream(String ruta, int n) {
        long inicio = System.nanoTime();
        try (BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream(ruta))) {
            for (int i = 0; i < n; i++) bos.write((i + "\n").getBytes());
        } catch (IOException e) { e.printStackTrace(); }
        long fin = System.nanoTime();
        System.out.printf("BufferedOutputStream escritura: %8.3f ms%n", (fin - inicio) / 1_000_000.0);
    }

    // ---------- Binario: lectura ----------
    private static void leerFileInputStream(String ruta) {
        long inicio = System.nanoTime();
        try (FileInputStream fis = new FileInputStream(ruta)) {
            while (fis.read() != -1) { }
        } catch (IOException e) { e.printStackTrace(); }
        long fin = System.nanoTime();
        System.out.printf("FileInputStream     lectura  : %8.3f ms%n", (fin - inicio) / 1_000_000.0);
    }

    private static void leerBufferedInputStream(String ruta) {
        long inicio = System.nanoTime();
        try (BufferedInputStream bis = new BufferedInputStream(new FileInputStream(ruta))) {
            while (bis.read() != -1) { }
        } catch (IOException e) { e.printStackTrace(); }
        long fin = System.nanoTime();
        System.out.printf("BufferedInputStream lectura  : %8.3f ms%n", (fin - inicio) / 1_000_000.0);
    }
}

## 2. Tablas de Resultados

### Resultados para n = 100,000
| Flujo utilizado | Tamaño | Ejec. 1 | Ejec. 2 | Ejec. 3 | Promedio |
| :--- | :--- | :--- | :--- | :--- | :--- |
| FileWriter | 100,000 | 52.764 | 49.316 | 52.633 | 51.57 |
| BufferedWriter | 100,000 | 19.574 | 15.786 | 13.885 | 16.42 |
| FileReader | 100,000 | 65.370 | 63.078 | 64.319 | 64.26 |
| BufferedReader | 100,000 | 10.763 | 9.694 | 13.145 | 11.20 |
| FileOutputStream | 100,000 | 370.501 | 373.756 | 419.279 | 387.85 |
| BufferedOutputStream | 100,000 | 15.177 | 13.221 | 17.528 | 15.31 |
| FileInputStream | 100,000 | 1128.457 | 1156.687 | 1146.233 | 1143.79 |
| BufferedInputStream | 100,000 | 16.100 | 15.951 | 19.579 | 17.21 |

### Resultados para n = 1,000,000
| Flujo utilizado | Tamaño | Ejec. 1 | Ejec. 2 | Ejec. 3 | Promedio |
| :--- | :--- | :--- | :--- | :--- | :--- |
| FileWriter | 1,000,000 | 223.424 | 211.169 | 202.611 | 212.40 |
| BufferedWriter | 1,000,000 | 81.666 | 96.993 | 74.467 | 84.38 |
| FileReader | 1,000,000 | 432.453 | 409.797 | 406.956 | 416.40 |
| BufferedReader | 1,000,000 | 55.861 | 58.142 | 50.346 | 54.78 |
| FileOutputStream | 1,000,000 | 5182.442 | 5065.374 | 5096.679 | 5114.83 |
| BufferedOutputStream | 1,000,000 | 80.165 | 82.761 | 77.679 | 80.20 |
| FileInputStream | 1,000,000 | 13308.580 | 13191.752 | 12771.835 | 13090.72 |
| BufferedInputStream | 1,000,000 | 160.217 | 169.464 | 160.144 | 163.27 |

### Resultados para n = 10,000,000
| Flujo utilizado | Tamaño | Ejec. 1 | Ejec. 2 | Ejec. 3 | Promedio |
| :--- | :--- | :--- | :--- | :--- | :--- |
| FileWriter | 10,000,000 | 2247.460 | 2388.792 | 3042.879 | 2559.71 |
| BufferedWriter | 10,000,000 | 571.096 | 752.047 | 942.555 | 755.23 |
| FileReader | 10,000,000 | 4721.780 | 4229.300 | 4132.184 | 4361.09 |
| BufferedReader | 10,000,000 | 485.907 | 404.528 | 391.114 | 427.18 |
| FileOutputStream | 10,000,000 | 60028.758 | 56876.131 | 51893.273 | 56266.05 |
| BufferedOutputStream | 10,000,000 | 1824.295 | 673.491 | 664.744 | 1054.18 |
| FileInputStream | 10,000,000 | 193518.419 | 142275.299 | 141480.859 | 159091.53 |
| BufferedInputStream | 10,000,000 | 1852.411 | 1804.614 | 1821.322 | 1826.12 |

## 3. Preguntas de Análisis

1. **¿Qué diferencia observaste entre usar flujos con buffer y los directos?**
La diferencia fue abismal. Los flujos con buffer fueron hasta 80 veces más rápidos. Por ejemplo, con 10 millones de datos, FileInputStream tardó un promedio de 159 segundos, mientras que BufferedInputStream tardó solo 1.8 segundos. Ocurre porque los flujos sin buffer hacen una llamada al sistema operativo por cada byte, lo cual es lento. Los flujos con buffer acumulan los datos en memoria RAM (bloques de 8 KB) y los leen de golpe.

2. **¿Cómo varía el tiempo de ejecución al aumentar n? ¿La relación es lineal?**
El tiempo aumenta de forma lineal. Al multiplicar por 10 el tamaño de n, el tiempo también se multiplicó por 10 en la mayoría de los casos. Se comporta igual en lectura que en escritura porque la cantidad de operaciones es proporcional a los datos.

3. **¿Existe diferencia de rendimiento entre caracteres (texto) y bytes (binario)?**
Sí, los flujos de texto son ligeramente más lentos porque tienen que codificar/decodificar caracteres (UTF-8). Sin embargo, la diferencia principal no es texto vs binario, sino usar o no buffer.

4. **¿Qué tamaño tiene el buffer por defecto y se puede cambiar?**
El tamaño por defecto es de 8192 bytes (8 KB). Sí se puede cambiar usando constructores sobrecargados, por ejemplo: `new BufferedWriter(new FileWriter("archivo.txt"), 16384);` para 16 KB.

5. **¿Por qué BufferedReader ofrece readLine() y BufferedInputStream no?**
Porque BufferedReader lee texto (caracteres) y entiende los saltos de línea. BufferedInputStream lee bytes crudos y no sabe qué es una línea de texto.

## 4. Conclusión
En esta práctica comprobé la inmensa importancia de usar buffers en Java. Leer o escribir sin buffer puede congelar el programa durante minutos, obligando al disco duro a trabajar ineficientemente. Al usar buffers, el rendimiento mejora exponencialmente porque se aprovecha la velocidad de la RAM. A partir de ahora, siempre usaré las clases Buffered para E/S.