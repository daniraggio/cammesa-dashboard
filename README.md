# CAMMESA Dashboard

Dashboard en tiempo real del Mercado Eléctrico Mayorista (MEM) de Argentina.

## Cómo funciona

```
GitHub Actions (cada 10 min)
  └── fetch api.cammesa.com  →  data/*.json  →  git push
                                                    ↓
                                            GitHub Pages sirve
                                            index.html + data/
```

- **Sin CORS**: los fetches los hace GitHub Actions server-side, no el navegador
- **Sin backend**: todo estático en GitHub Pages
- **Sin dependencias**: HTML + Chart.js desde CDN

## Setup (5 minutos)

### 1. Crear el repo

```bash
git init cammesa-dashboard
cd cammesa-dashboard
# copiar los archivos de este repo
git add .
git commit -m "initial"
git remote add origin https://github.com/TU_USUARIO/cammesa-dashboard.git
git push -u origin main
```

### 2. Activar GitHub Pages

- Ir a **Settings → Pages**
- Source: `Deploy from a branch`
- Branch: `main` / `/ (root)`
- Guardar

### 3. Permitir que Actions escriba en el repo

- Ir a **Settings → Actions → General**
- Bajar hasta "Workflow permissions"
- Seleccionar **"Read and write permissions"**
- Guardar

### 4. Correr el workflow por primera vez

- Ir a **Actions → Fetch CAMMESA Data → Run workflow**

Listo. El dashboard queda en `https://TU_USUARIO.github.io/cammesa-dashboard/`

## Datos disponibles

| Archivo | Fuente | Frecuencia |
|---|---|---|
| `data/costo_marginal.json` | costoMarginal (hoy) | cada 10 min |
| `data/costo_marginal_mes.json` | costoMarginal (mes) | cada 10 min |
| `data/precio_mercado_pd.json` | precioMercadoPD (hoy) | cada 10 min |
| `data/precio_mercado_def.json` | precioMercadoDef (mes) | cada 10 min |
| `data/embalses.json` | cuencasDatosHidraulicos | cada 10 min |
| `data/consumo_gas.json` | consumoGas (mes) | cada 10 min |
| `data/perturbaciones.json` | perturbacionesSadiTodas (7 días) | cada 10 min |
| `data/meta.json` | timestamp | cada 10 min |

## Notas

- GitHub Actions tiene un límite de **~2000 minutos/mes** en cuentas free. Corriendo cada 10 min = ~4320 ejecuciones/mes × ~20 seg = ~1440 minutos/mes. Está justo en el límite; si querés margen, cambiá el cron a `*/15 * * * *`.
- Los datos de `demanda-svc` (demanda real, generación, corredores) no están incluidos porque ese endpoint bloquea CORS y tampoco tiene documentación pública de autenticación.
