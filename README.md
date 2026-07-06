# pydantic_core para Termux (Android, Python 3.14)

Compila `pydantic_core` para Termux/Android usando GitHub Actions (gratis),
evitando compilar en el teléfono (que falla por falta de RAM).

## Cómo usar

1. Sube este repo a GitHub (público o privado, funciona igual).
2. Ve a la pestaña **Actions** de tu repo en GitHub.
3. Selecciona el workflow **"Build pydantic_core wheel para Termux"**.
4. Click en **"Run workflow"**.
5. En el campo `pydantic_core_version`, pon la versión EXACTA que tu
   `pydantic` está pidiendo. Para saber cuál es, en tu Termux corre:

   ```bash
   pip show pydantic | grep Version
   ```

   Si tienes pydantic 2.13.4, normalmente pide pydantic_core==2.46.4
   (puedes confirmar el número exacto en el traceback de tu error anterior,
   donde decía `pydantic-core==2.46.4`).

6. Dale a "Run workflow" (botón verde) y espera ~10-15 minutos.
7. Cuando termine (ícono verde ✓), ve a la sección **Releases** de tu repo.
8. Ahí vas a encontrar el archivo `.whl` — descárgalo a tu teléfono.
9. En Termux, dentro de la carpeta donde lo descargaste:

   ```bash
   pip install pydantic_core-*.whl --no-deps
   python3 -c "import pydantic_core; print('ok:', pydantic_core.__version__)"
   ```

10. Si eso funciona, reintenta:

    ```bash
    python3 -c "from blockrun_llm import LLMClient; print('ok')"
    ```

## Si falla la compilación en GitHub Actions

Este es un intento razonado basado en la técnica de
[Eutalix/android-pydantic-core](https://github.com/Eutalix/android-pydantic-core)
(que cubre hasta Python 3.13), adaptado a 3.14. Puede necesitar ajustes si:

- El nombre del NDK API level (`android24`) no coincide con tu build de Termux.
- La versión de `cargo-ndk` o `maturin` cambia su forma de generar el tag del wheel.
- El linker no encuentra `libpython3.14.so` (en ese caso, revisa el nombre exacto
  de esa librería en tu Termux: `find /data/data/com.termux/files/usr/lib -name "libpython*"`).

Si algo de esto falla, copia el log de error de la pestaña Actions y compártelo
para ajustar el workflow.
