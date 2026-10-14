# Heritage Treasures: An In-Depth Analysis of UNESCO World Heritage Sites in Tableau

An interactive Tableau dashboard analyzing UNESCO World Heritage Sites through geographical distribution, site categories, countries, cultural significance, and preservation insights using data visualization.

## 📌 Project Overview

UNESCO World Heritage Sites represent some of the world's most significant cultural and natural landmarks, but information about them is often scattered across spreadsheets and static reports. This project consolidates that data into an interactive dashboard suite — covering site distribution, geography, trends over time, and conservation risk — published on Tableau Public and embedded in a custom Flask web application.

## 🎯 Problem Statement

Researchers, conservation organizations, students, and travelers lack a single, visual, interactive way to explore UNESCO heritage data — making it hard to quickly identify at-risk sites or understand global trends.

## ✨ Features

- **8 interactive visualizations** across 2 dashboards:
  - Categories by Sites Count (bar chart)
  - Site Count per Region (pie chart)
  - Top 10 Regions by Area (map)
  - Year Forecasting of Heritages (line chart)
  - Heritage Sites by Country (treemap)
  - Countries per Region (bubble chart)
  - Heritage Sites at Risk — In Danger vs Not (pie chart)
  - Regional Inscription Trends (line chart)
- **Guided Tableau Story** walking through both dashboards
- **Flask web application** embedding the dashboards with a dark/light mode toggle
- Published live on **Tableau Public**

## 🛠️ Tools & Technologies Used

| Category | Tool / Technology |
|---|---|
| Data Visualization | Tableau Desktop, Tableau Public |
| Backend | Python, Flask |
| Frontend | HTML5, CSS3, JavaScript |
| Frontend Frameworks | Bootstrap 5, Animate.css, AOS (Animate on Scroll) |
| Maps | Mapbox / OpenStreetMap (via Tableau) |
| Data Preparation | Excel, Tableau Data Prep |
| Version Control | Git, GitHub |
| Code Editor | Visual Studio Code |

## 📊 Live Dashboards

- Dashboard 1 (Overview): https://public.tableau.com/app/profile/molla.afrid/viz/UNESCOStory_17833528420560/Dashboard1
- Dashboard 2 (Danger Analysis): https://public.tableau.com/app/profile/molla.afrid/viz/UNESCODangerSItesOverview/Dashboard2
- Story: https://public.tableau.com/app/profile/molla.afrid/viz/UNESCOStory_17833528420560/Story1
- Tableau Public Profile: https://public.tableau.com/app/profile/molla.afrid/vizzes

## 🎥 Project Demo

https://drive.google.com/file/d/1Ry7an2m_XbNJmHX_ke2lxzmLZ-59-qAu/view

## 💻 Source Code

The complete Flask web application source code below (`app.py` and `templates/index.html`) embeds the Tableau dashboards into a custom web page. This code is also included in the **Final Report** document (Appendix / Web Integration section).

### `app.py`

```python
from flask import *

app = Flask(__name__)

@app.route("/")
def home():
    return render_template("index.html")

if __name__ == "__main__":
    app.run(debug=True, port=1212)
```

### `templates/index.html`

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>UNESCO Global Heritage Explorer</title>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet" />
  <link href="https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css" rel="stylesheet" />
  <link href="https://unpkg.com/aos@2.3.1/dist/aos.css" rel="stylesheet" />
  <style>
    :root {
      --primary-gold: #c5a059;
      --deep-navy: #0f172a;
      --slate-800: #1e293b;
      --bg-light: #f8fafc;
      --text-main: #334155;
    }

    body {
      background: var(--bg-light);
      background-image: radial-gradient(#cbd5e1 0.5px, transparent 0.5px);
      background-size: 24px 24px;
      color: var(--text-main);
      font-family: 'Inter', system-ui, -apple-system, sans-serif;
      transition: all 0.4s ease;
      scroll-behavior: smooth;
    }

    .dark-mode {
      background: var(--deep-navy);
      background-image: radial-gradient(#334155 0.5px, transparent 0.5px);
      color: #f1f5f9;
    }

    .navbar {
      background: rgba(15, 23, 42, 0.9) !important;
      backdrop-filter: blur(10px);
      border-bottom: 1px solid var(--primary-gold);
    }

    .header {
      text-align: center;
      margin-top: 2rem;
      margin-bottom: 4rem;
      padding: 4rem 2rem;
      background: linear-gradient(135deg, #0f172a 0%, #1e293b 100%);
      color: white;
      border-radius: 2rem;
      border: 1px solid rgba(197, 160, 89, 0.3);
      box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.5);
    }

    .header h1 {
      font-weight: 800;
      color: var(--primary-gold);
      text-transform: uppercase;
      letter-spacing: -1px;
    }

    .tableau-card {
      background: rgba(255, 255, 255, 0.9);
      backdrop-filter: blur(12px);
      border-radius: 1.5rem;
      border: 1px solid rgba(226, 232, 240, 1);
      box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
      padding: 2rem;
      margin-bottom: 4rem;
      transition: all 0.3s ease;
    }

    .dark-mode .tableau-card {
      background: rgba(30, 41, 59, 0.8);
      border-color: rgba(197, 160, 89, 0.2);
    }

    .tableau-card:hover {
      transform: translateY(-5px);
      box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.2);
    }

    .title-bar {
      font-size: 1.8rem;
      font-weight: 700;
      margin-bottom: 2rem;
      color: var(--deep-navy);
      border-left: 5px solid var(--primary-gold);
      padding-left: 1rem;
    }

    .dark-mode .title-bar { color: var(--primary-gold); }

    .scroll-indicator {
      position: fixed;
      top: 0; left: 0; height: 5px;
      background: var(--primary-gold);
      z-index: 10000;
    }

    footer {
      background: var(--deep-navy);
      border-top: 2px solid var(--primary-gold);
      padding: 3rem;
      color: white;
      margin-top: 5rem;
    }
  </style>
</head>
<body>
  <div class="scroll-indicator" id="scrollIndicator"></div>

  <nav class="navbar navbar-expand-lg sticky-top navbar-dark">
    <div class="container">
      <a class="navbar-brand fw-bold" href="#">UNESCO <span style="color:var(--primary-gold)">VISUALIZER</span></a>
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarNav">
        <ul class="navbar-nav ms-auto align-items-center">
          <li class="nav-item"><a class="nav-link" href="#dashboard1">Overview</a></li>
          <li class="nav-item"><a class="nav-link" href="#dashboard2">Danger Analysis</a></li>
          <li class="nav-item"><a class="nav-link" href="#story">The Story</a></li>
          <li class="nav-item"><button class="btn btn-outline-warning ms-lg-3" onclick="toggleMode()">🌓 Toggle Mode</button></li>
        </ul>
      </div>
    </div>
  </nav>

  <div class="container py-4">
    <div class="header animate__animated animate__fadeIn">
      <h1 class="display-3">World Heritage Treasures</h1>
      <p class="lead opacity-75">Visualizing the preservation of human history and natural wonder</p>
    </div>

    <div class="tableau-card" id="dashboard1" data-aos="fade-up">
      <div class="title-bar">Site Distribution Overview</div>
      <div class='tableauPlaceholder' id='viz1783755341395'>
        <object class='tableauViz' style='display:none;'>
          <param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F'/>
          <param name='embed_code_version' value='3'/>
          <param name='name' value='UNESCOStory_17833528420560/Dashboard1'/>
          <param name='toolbar' value='yes'/>
        </object>
      </div>
    </div>

    <div class="tableau-card" id="dashboard2" data-aos="fade-up">
      <div class="title-bar">Vulnerability & Danger Analysis</div>
      <div class='tableauPlaceholder' id='viz1783755383964'>
        <object class='tableauViz' style='display:none;'>
          <param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F'/>
          <param name='embed_code_version' value='3'/>
          <param name='name' value='UNESCODangerSItesOverview/Dashboard2'/>
          <param name='toolbar' value='yes'/>
        </object>
      </div>
    </div>

    <div class="tableau-card" id="story" data-aos="fade-up">
      <div class="title-bar">UNESCO Heritage Story</div>
      <div class='tableauPlaceholder' id='viz1783755497441'>
        <object class='tableauViz' style='display:none;'>
          <param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F'/>
          <param name='embed_code_version' value='3'/>
          <param name='name' value='UNESCOStory_17833528420560/Story1'/> 
          <param name='toolbar' value='yes'/>
          <param name='display_count' value='yes'/>
          <param name='language' value='en-US'/>
        </object>
      </div>
    </div>
  </div>

  <footer class="text-center">
    <p class="mb-0 fw-light">Data Source: UNESCO World Heritage List</p>
    <p class="small opacity-50">&copy; 2026 Digital Heritage Project | Tableau Public Integration</p>
  </footer>

  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
  <script src="https://unpkg.com/aos@2.3.1/dist/aos.js"></script>
  <script>
    AOS.init({ duration: 800, once: true });

    const loadTableau = (id) => {
      const div = document.getElementById(id);
      const viz = div.getElementsByTagName('object')[0];
      if (viz) {
        viz.style.display = 'block';
        viz.style.width = '100%';
        viz.style.height = window.innerWidth < 768 ? '600px' : '850px';
        const script = document.createElement('script');
        script.src = 'https://public.tableau.com/javascripts/api/viz_v1.js';
        viz.parentNode.insertBefore(script, viz);
      }
    };

    window.onload = () => {
      loadTableau('viz1783755341395');
      loadTableau('viz1783755383964');
      loadTableau('viz1783755497441');
    };

    function toggleMode() { document.body.classList.toggle('dark-mode'); }

    window.onscroll = () => {
      const winScroll = document.documentElement.scrollTop;
      const height = document.documentElement.scrollHeight - document.documentElement.clientHeight;
      document.getElementById("scrollIndicator").style.width = (winScroll / height) * 100 + "%";
    };
  </script>
</body>
</html>
```

## ▶️ How to Run the Web App Locally

1. Make sure Python is installed on your machine.
2. Install Flask:
```
   pip install flask
```
3. Save the `app.py` code above into a file named `app.py`.
4. Create a folder named `templates` in the same location as `app.py`, and save the `index.html` code above inside it as `templates/index.html`.
5. Open a terminal in that folder and run:
```
   python app.py
```
6. You should see output like:
```
   * Running on http://127.0.0.1:1212
```
7. Open your browser and go to:
```
   http://127.0.0.1:1212
```
8. The website will load with the embedded Tableau dashboards, danger analysis, and story.

## 👥 Team

- Konda Prabhakar — Team Lead
- Madarapu Sri Venkata Krishna - Member
- Molla Afrid - Member
- Anuradha Malleswarapu - Member
- Madduluri Sushma Sree - Member
