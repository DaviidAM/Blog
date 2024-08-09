# Mejorar la apariencia de la terminal

[Enlace al video](https://www.youtube.com/watch?v=wNQpDWLs4To)

1. Instala `iterm2` desde la página web: [enlace](https://iterm2.com)

2. Configura zsh como la terminal predeterminada

```bash
# Verifica si zsh está instalado
which zsh

# Si no está instalado, instálalo  
# ...

# Asigna zsh como la terminal predeterminada
chsh -s $(which zsh)

# Después de abrir una nueva terminal, la predeterminada debería ser zsh
echo $0
```

3. Configura los colores para la terminal

```bash
nano ~/gruvbox.itermcolors
```

con el texto del [enlace](https://github.com/herrbischoff/iterm2-gruvbox/blob/master/gruvbox.itermcolors)

5. Instala oh my zsh

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

6. Descarga la fuente que deseas desde [enlace](https://www.nerdfonts.com/font-downloads)

- Hack Nerd Font
- Haz doble clic e instala una fuente

7. En la configuración de la nueva terminal, ve a profiles > text y selecciona la fuente.

9. Instala Powerlevel 10K -> [enlace](https://github.com/romkatv/powerlevel10k)

```shell
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

9. Configura ZSH_THEME="powerlevel10k/powerlevel10k" en ~/.zshrc.

10. Recarga la terminal

```bash
source `~/.zshrc`
```

11. Configura la terminal a tu gusto.
12. Instala algunos plugins extras
 1. `zsh-syntax-highlighting`  -> [link](https://github.com/zsh-users/zsh-syntax-highlighting)

  ```bash
  git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
  ```

 2. `zsh-autosuggestions` -> [link](https://github.com/zsh-users/zsh-autosuggestions)

  ```bash
  git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
  ```

12. Agrega los nuevos plugins dentro de zsh

```bash
nano ~/.zshrc
```

y modifica la línea de plugins:

```
plugins=(git zsh-syntax-highlighting zsh-autosuggestions)
```

13. Recarga la terminal

```bash
source `~/.zshrc`
```
