# Inventario y comprobación del entorno

Utiliza esta tabla para relacionar cada máquina virtual con sus adaptadores. Los datos exactos dependerán del escenario.

| Máquina | Sistema | Tipo | Interfaz o adaptador | MAC | Comprobada |
|---|---|---|---|---|---|
|  |  | Madre / clon |  |  | Sí / No |
|  |  | Madre / clon |  |  | Sí / No |
|  |  | Madre / clon |  |  | Sí / No |

## Cómo localizar la MAC

En VirtualBox, consulta la configuración avanzada del adaptador de red.

En Linux:

```bash
ip link show
```

En PowerShell:

```powershell
Get-NetAdapter
```

La MAC mostrada por el sistema debe coincidir con la del adaptador correspondiente en VirtualBox.

## Comprobación final

- [ ] Las tres máquinas madre arrancan y permiten iniciar sesión.
- [ ] Las máquinas madre están apagadas antes de crear los clones.
- [ ] Existe una instantánea de referencia con un nombre reconocible.
- [ ] Cada sistema dispone de un clon enlazado de trabajo.
- [ ] Los clones arrancan correctamente.
- [ ] Las máquinas que funcionarán simultáneamente tienen MAC distintas.
- [ ] Se puede distinguir cada máquina por su nombre.
- [ ] No se ha trabajado directamente sobre la máquina madre.

Cuando todos los puntos estén comprobados, continúa con [Red NAT y Host-Only](./03_red_nat_y_hostonly.md).
