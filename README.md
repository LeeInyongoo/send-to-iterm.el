# send-to-iterm2

This package enables simple communication with a gui emacs (Emacs.app or
Aquamacs.app) and the Mac OS X app [iTerm2](https://www.iterm2.com/). 
This package is a modified version of the code in this [gist](https://gist.github.com/johnmastro/88cc318f4ce33b626c9d).

It defines the following commands:

## General Commands
* `iterm-send-text` - send current line or highlighted region to iTerm
* `iterm-cd` - sends "cd [current file's directory]" to iTerm
## Python Commands
* `iterm-send-text-ipy` - sends current line or highlighted region to iTerm
    using ipython %cpaste magic
* `iterm-send-file-ipy` - sends "%run [current file]" to iTerm
* `iterm-cwd-ipy` - sends "%cd [current file's directory]" to iTerm
## R Commands
* `iterm-send-file-R` - sends "source([current file])" to iTerm
* `iterm-cwd-R` - sends "setwd([current file's directory])" to iTerm
## Julia Commands
* `iterm-send-file-julia` - sends "include([current file])" to iTerm
* `iterm-cwd-juli` - sends "cd([curent file's directory])" to iTerm

Pull requests for other, language specific commands are welcome.

# Installation & Configuration

I use [use-package](https://github.com/jwiegley/use-package) to install and
configure iterm.el with the following lines in my `.emacs` file.

NOTE: If you use Aquamacs.app (instead of Emacs.app) you will have to change the
"s-" key code to an "A-" keycode.

```elisp
(use-package iterm
  :load-path [directory of iterm.el]
  :ensure t
  :commands (iterm-send-text-ipy
             iterm-send-file-ipy
             iterm-cwd-ipy
             iterm-send-file-R
             iterm-cwd-R
             iterm-send-file-julia
             iterm-cwd-julia)
  :bind (("<s-return>" . iterm-send-text)
         ("C-s-/" . iterm-cd)))

(use-package
  python :ensure t
  :mode (("\\.py\\'" . python-mode))
  :config
  (add-hook 'python-mode-hook
            (lambda ()
              (local-set-key (kbd "<s-return>") 'iterm-send-text-ipy)
              (local-set-key (kbd "s-b") 'iterm-send-file-ipy)
              (local-set-key (kbd "s-/") 'iterm-cwd-ipy))))
              

(use-package ess-site :ensure t
  :mode (("\\.R\\'" . ess-r-mode)
         ("\\.jl\\'" . ess-julia-mode))
  :config
  (require 'ess-site)

  ;; R
  (add-hook 'ess-mode-hook
            (lambda ()
              (local-set-key (kbd "s-b") 'iterm-send-file-R)
              (local-set-key (kbd "s-/") 'iterm-cwd-R)))

  ;; julia
  (add-hook 'ess-julia-mode-hook
            (lambda ()
              (local-set-key (kbd "s-b") 'iterm-send-file-julia)
              (local-set-key (kbd "s-/") 'iterm-cwd-julia))))
              
```
