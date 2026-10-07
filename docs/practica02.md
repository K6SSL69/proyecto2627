# Cálculo del salario según el puesto

Este código permite introducir un **sueldo** y seleccionar un **puesto de trabajo**.
Después, los datos se envían mediante `POST` a un archivo PHP, que calcula un complemento según el puesto y muestra el sueldo final.

## 1. Formulario HTML

El formulario recoge dos datos:

* **Sueldo:** cantidad introducida por el usuario.
* **Puesto:** puesto seleccionado de una lista.

Los datos se envían mediante el método `POST` al archivo `ut02p02.php`.

```html
<form name="myform" method="post" action="ut02p02.php">
        
    <input
        class="input is-link"
        type="number"
        placeholder="Introduce tu sueldo: "
        name="sueldo"
        min="1001" 
        required
    />

    <br>

    <label for="puesto">PUESTO:</label>
    <select id="puesto" name="puesto" required>
        <option value="">-- Selecciona un puesto --</option>
        <option value="base">Base</option>
        <option value="directivo">Directivo</option>
        <option value="alto_cargo">Alto cargo</option>
    </select>

    <br>

    <input type="submit" class="button is-warning" value="CALCULAR SALARIO">

</form>
```

### ¿Qué hace cada parte?

* `method="post"` → indica que los datos se enviarán mediante `POST`.
* `action="ut02p02.php"` → indica el archivo PHP que recibirá los datos.
* `name="sueldo"` → nombre utilizado para recoger el sueldo en PHP.
* `name="puesto"` → nombre utilizado para recoger el puesto en PHP.
* `min="1001"` → obliga a introducir un sueldo mínimo de 1001 €.
* `required` → hace que el campo sea obligatorio.
* `<select>` → permite seleccionar uno de los puestos disponibles.
* `type="submit"` → crea el botón que envía el formulario.

---

## 2. Código PHP

El archivo PHP recibe los datos enviados por el formulario utilizando `$_POST`.

```php
<?php
    $sueldo = $_POST["sueldo"];
    $puesto = $_POST["puesto"];

    switch ($puesto) {

        case "base":
            $porcentaje = 10;
            break;

        case "directivo":
            $porcentaje = 15;
            break;

        case "alto_cargo":
            $porcentaje = 20;
            break;

        default:
            die("Puesto no válido.");
    }

    $complemento = $sueldo * ($porcentaje / 100);

    $sueldoFinal = $sueldo + $complemento;
?>
```

### Recoger los datos

Con `$_POST` obtenemos los valores enviados desde el formulario:

```php
$sueldo = $_POST["sueldo"];
$puesto = $_POST["puesto"];
```

* `$_POST["sueldo"]` → recoge el sueldo introducido.
* `$_POST["puesto"]` → recoge el puesto seleccionado.

---

### Elegir el porcentaje

Se utiliza un `switch` para comprobar el puesto seleccionado y establecer el porcentaje correspondiente:

```php
switch ($puesto) {

    case "base":
        $porcentaje = 10;
        break;

    case "directivo":
        $porcentaje = 15;
        break;

    case "alto_cargo":
        $porcentaje = 20;
        break;

    default:
        die("Puesto no válido.");
}
```

Los porcentajes son:

| Puesto     | Complemento |
| ---------- | ----------: |
| Base       |         10% |
| Directivo  |         15% |
| Alto cargo |         20% |

Si el puesto no coincide con ninguno de los casos, se ejecuta:

```php
die("Puesto no válido.");
```

Esto detiene la ejecución y muestra un mensaje de error.

---

## 3. Calcular el complemento

Una vez conocemos el sueldo y el porcentaje, calculamos el complemento:

```php
$complemento = $sueldo * ($porcentaje / 100);
```

Por ejemplo, si el sueldo es **2000 €** y el porcentaje es **15%**:

```text
2000 × (15 / 100) = 300 €
```

Por tanto, el complemento sería de **300 €**.

---

## 4. Calcular el sueldo final

Después sumamos el sueldo base y el complemento:

```php
$sueldoFinal = $sueldo + $complemento;
```

Siguiendo el ejemplo anterior:

```text
2000 + 300 = 2300 €
```

El sueldo final sería **2300 €**.

---

## 5. Mostrar el resultado

Finalmente, PHP muestra los resultados utilizando `echo`:

```php
<h1>Resultado del salario</h1>

<p>
    El sueldo base es de
    <?php echo $sueldo; ?>€
</p>

<p>
    El complemento es del
    <?php echo $porcentaje; ?>%
</p>

<p>
    El sueldo final es de
    <?php echo $sueldoFinal; ?>€
</p>
```

`echo` permite mostrar en HTML el valor de una variable PHP.