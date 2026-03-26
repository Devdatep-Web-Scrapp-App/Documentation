# DOCS



## Diseño Base

[![initial-dashboard-idea.png](https://i.postimg.cc/cHrHpJQ8/initial-dashboard-idea.png)](https://postimg.cc/FdXNcNC9)


## Requisitos Iniciales

1.Seguidores diarios / tendencia (rango de fecha)

2.Cual rrss creció más en el rango de fecha (grafico de barras)

3.Qué día subimos más seguidores en una red social en específico.

4.Tabla resumen para saber hasta qué día hay información

5.Saber qué red social se ah quedado estancada de acuerdo en x días (filtro de rango de fechas).(no tuvo incremento / incremento mínimo)

6.Saber qué red social subió más de acuerdo en x días (lo contrario al 5)

7.Qué día subió más seguidores de acuerdo a un rango de fecha por red social.


## Arquitectura Planteada

<style>
.tree { font-family: var(--font-mono); font-size: 13px; color: var(--color-text-primary); line-height: 1.9; padding: 16px 20px; }
.dir  { color: var(--color-text-primary); font-weight: 500; }
.file { color: var(--color-text-secondary); }
.badge { display: inline-block; font-size: 10px; font-weight: 500; padding: 1px 6px; border-radius: 4px; margin-left: 8px; vertical-align: middle; }
.b-teal   { background: #E1F5EE; color: #0F6E56; }
.b-purple { background: #EEEDFE; color: #3C3489; }
.b-amber  { background: #FAEEDA; color: #633806; }
.b-blue   { background: #E6F1FB; color: #0C447C; }
.b-coral  { background: #FAECE7; color: #712B13; }
@media (prefers-color-scheme: dark) {
  .b-teal   { background: #085041; color: #9FE1CB; }
  .b-purple { background: #26215C; color: #CECBF6; }
  .b-amber  { background: #412402; color: #FAC775; }
  .b-blue   { background: #042C53; color: #B5D4F4; }
  .b-coral  { background: #4A1B0C; color: #F5C4B3; }
}
</style>
<div class="tree">
<span class="dir">rrss-dashboard-app/</span><br>
│<br>
├── <span class="dir">backend/</span> <span class="badge b-teal">FastAPI</span><br>
│   ├── <span class="dir">app/</span><br>
│   │   ├── <span class="file">main.py</span> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:var(--color-text-tertiary);font-size:12px">— entry point, CORS, routers</span><br>
│   │   ├── <span class="file">config.py</span> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:var(--color-text-tertiary);font-size:12px">— env vars, DB settings</span><br>
│   │   ├── <span class="file">database.py</span> &nbsp;&nbsp;&nbsp;<span style="color:var(--color-text-tertiary);font-size:12px">— MySQL pool (reusar tu lógica)</span><br>
│   │   ├── <span class="dir">routers/</span><br>
│   │   │   ├── <span class="file">auth.py</span> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:var(--color-text-tertiary);font-size:12px">— register, login, JWT</span><br>
│   │   │   ├── <span class="file">stats.py</span> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:var(--color-text-tertiary);font-size:12px">— followers, lost, history</span><br>
│   │   │   ├── <span class="file">scraper.py</span> &nbsp;&nbsp;<span style="color:var(--color-text-tertiary);font-size:12px">— trigger scrape, status</span><br>
│   │   │   └── <span class="file">settings.py</span> &nbsp;<span style="color:var(--color-text-tertiary);font-size:12px">— guardar IG credentials</span><br>
│   │   ├── <span class="dir">models/</span><br>
│   │   │   ├── <span class="file">user.py</span> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:var(--color-text-tertiary);font-size:12px">— User SQLAlchemy model</span><br>
│   │   │   └── <span class="file">instagram.py</span> &nbsp;<span style="color:var(--color-text-tertiary);font-size:12px">— Snapshot, Lost, History</span><br>
│   │   ├── <span class="dir">schemas/</span> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:var(--color-text-tertiary);font-size:12px">— Pydantic request/response</span><br>
│   │   ├── <span class="dir">services/</span><br>
│   │   │   ├── <span class="file">auth_service.py</span><br>
│   │   │   └── <span class="file">ig_scraper.py</span> &nbsp;<span style="color:var(--color-text-tertiary);font-size:12px">— tu script adaptado</span><br>
│   │   └── <span class="dir">tasks/</span><br>
│   │       └── <span class="file">celery_app.py</span> &nbsp;<span style="color:var(--color-text-tertiary);font-size:12px">— worker + scheduler</span><br>
│   ├── <span class="file">.env</span><br>
│   └── <span class="file">requirements.txt</span><br>
│<br>
├── <span class="dir">frontend/</span> <span class="badge b-purple">React</span><br>
│   ├── <span class="dir">src/</span><br>
│   │   ├── <span class="dir">pages/</span><br>
│   │   │   ├── <span class="file">Login.jsx</span><br>
│   │   │   ├── <span class="file">Dashboard.jsx</span><br>
│   │   │   ├── <span class="file">History.jsx</span><br>
│   │   │   └── <span class="file">Settings.jsx</span><br>
│   │   ├── <span class="dir">components/</span><br>
│   │   │   ├── <span class="file">FollowerChart.jsx</span><br>
│   │   │   ├── <span class="file">LostTable.jsx</span><br>
│   │   │   └── <span class="file">ScrapeStatus.jsx</span><br>
│   │   ├── <span class="dir">api/</span> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:var(--color-text-tertiary);font-size:12px">— axios calls al backend</span><br>
│   │   └── <span class="dir">context/</span> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:var(--color-text-tertiary);font-size:12px">— AuthContext (JWT)</span><br>
│   └── <span class="file">package.json</span><br>
│<br>
└── <span class="file">docker-compose.yml</span> <span class="badge b-blue">opcional</span>
</div>

