# Frontend-Py Subagent - Complete User Guide

**Date**: 2025-11-25  
**Status**: Active & Production-Ready  
**Repository**: sigamani/doubleword-technical

---

## Quick Start

### Activate Frontend-Py

```bash
/spawn frontend-py <task description>
```

### Example Tasks

**1. Build a Real-Time Monitoring Dashboard**
```bash
/spawn frontend-py Build a Streamlit dashboard displaying real-time metrics: throughput (req/s), latency (p50/p99), memory usage, and queue depth. Include filters for time range and model selection.
```

**2. Create a Model Inference UI**
```bash
/spawn frontend-py Create a Gradio interface for Qwen2.5-0.5B model inference with text input, example prompts, and formatted text output display.
```

**3. Implement an Operator Control Panel**
```bash
/spawn frontend-py Build a Streamlit operator panel for Ray Data job management with job status display, parameter controls, and log viewer.
```

**4. Add Analytics Dashboard**
```bash
/spawn frontend-py Build a multi-page Dash dashboard with: Page 1 - Overview metrics, Page 2 - Performance trends, Page 3 - Configuration analyzer.
```

---

## Subagent Capabilities

### Supported Frameworks

| Framework | Best For | Complexity | Development Speed |
|--|--|--|--|
| **Streamlit** | Internal tools, rapid prototyping, diagnostics | Low | Very Fast |
| **Plotly Dash** | Multi-page dashboards, real-time monitoring | Medium | Fast |
| **Gradio** | ML demo UIs, model inference interfaces | Low | Very Fast |
| **FastAPI + Jinja2/HTMX** | High-performance backends, server-driven UX | High | Medium |

### Enabled Tools

✓ **Python** (3.10+, full development environment)  
✓ **Package Management** (pip, dependency resolution)  
✓ **Version Control** (Git, commit/push/pull)  
✓ **Testing** (pytest, unit and integration tests)  
✓ **Node.js** (for build tools if needed)  
✓ **Documentation Access** (Perplexity Search MCP)  

### Security Constraints

✓ **Scope Lock**: Operates only within sigamani/doubleword-technical  
✓ **External Access**: Denied (configuration-enforced)  
✓ **No Secrets**: Credentials handled via environment variables  
✓ **No Speculation**: Only implements needed features  

---

## Workflow Sequence

Frontend-Py executes tasks in this strict order:

```
1. DISCOVER REQUIREMENTS
   ↓ Identify user-facing needs, map existing UIs, prioritize features
   
2. SELECT FRAMEWORK
   ↓ Choose optimal framework (Dash/Streamlit/Gradio/FastAPI)
   
3. BUILD/UPDATE COMPONENTS
   ↓ Design modular architecture, separate concerns
   
4. VALIDATE LOCALLY
   ↓ Run the app, verify no console errors, components render
   
5. TEST INTEGRATION
   ↓ Data flows end-to-end, callbacks work, performance acceptable
   
6. REPAIR & FIX
   ↓ Fix UI code AND dependent backends if issues found
   
7. DOCUMENT & REPORT
   ↓ Provide usage instructions, architecture, deployment guidance
```

---

## Design Philosophy

### Minimal Complexity
- Choose the lightest framework that solves the problem
- Never add heavy dependencies speculatively
- Question the necessity of each feature

### Performance-First
- Target <2 second interactions
- Lazy-load data when appropriate
- Cache intelligently (client-side, server-side)
- Monitor performance continuously

### Modularity & Reusability
- Build reusable components
- Separate data, rendering, and layout concerns
- Composition over monoliths
- Clean interfaces between modules

### Accessibility
- ARIA labels and semantic HTML
- Keyboard navigation support
- Color contrast compliance (WCAG AA)
- Graceful error messages

### Consistency
- Unified visual design language
- Consistent interaction patterns
- Predictable behavior
- Clear feedback to user actions

---

## Framework Selection Guide

### Choose Streamlit When:
- Building internal/admin tools
- Rapid prototyping needed
- Single-page dashboard sufficient
- Team values quick iteration over performance
- Data exploration/diagnostic UI needed

**Example**: Batch inference diagnostics, parameter exploration, log viewer

### Choose Plotly Dash When:
- Multi-page dashboard required
- Real-time state management needed
- Complex interactions/filters
- Professional appearance required
- Sophisticated visualizations needed

**Example**: Production monitoring dashboard, analytics portal

### Choose Gradio When:
- ML model demo needed
- Simple inference UI needed
- Quick sharing required (Hugging Face Spaces)
- Minimal frontend knowledge needed
- Single interface focus

**Example**: Model inference demo, quick POC

### Choose FastAPI + Jinja2/HTMX When:
- High-performance backend required
- Server-driven rendering preferred
- Minimal JavaScript needed
- API-first architecture
- Custom styling/branding needed

**Example**: Production API with custom frontend, hybrid rendering

---

## Development Workflow

### Step 1: Local Development
```bash
# Streamlit
streamlit run app.py

# Dash
python app.py

# Gradio
python app.py

# FastAPI
uvicorn app:app --reload
```

### Step 2: Testing
```bash
# Unit tests
pytest tests/ -v

# Code style
ruff check .

# Type checking (if applicable)
mypy app.py
```

### Step 3: Manual Validation
- [ ] App starts without errors
- [ ] All components render
- [ ] Interactions work correctly
- [ ] Data flows end-to-end
- [ ] Performance acceptable (<2s)
- [ ] Error handling graceful
- [ ] Responsive design works

### Step 4: Deploy
```bash
# Build Docker image
docker build -t batch-inference-dashboard .

# Run container
docker run -p 8501:8501 batch-inference-dashboard

# Or use docker-compose
docker-compose up
```

---

## Validation Checklist

Before declaring UI feature complete:

### Functionality
- [ ] App starts (`streamlit run` / `python app.py` / `uvicorn app:app`)
- [ ] All components render correctly (buttons, inputs, charts)
- [ ] Callbacks execute without console errors
- [ ] Data flows correctly from backend → UI → backend
- [ ] Forms submit successfully
- [ ] Filtering/sorting/searching works

### Performance
- [ ] Initial load time <2 seconds
- [ ] Interactions feel responsive (<200ms response)
- [ ] Charts render smoothly with large datasets
- [ ] No memory leaks during extended use
- [ ] Pagination/lazy-loading for large datasets

### User Experience
- [ ] Error messages are clear and helpful
- [ ] No stack traces exposed to user
- [ ] Graceful degradation on errors
- [ ] Loading states are obvious
- [ ] Consistent visual design

### Accessibility
- [ ] Keyboard navigation works
- [ ] ARIA labels present
- [ ] Color contrast WCAG AA compliant
- [ ] Readable font sizes
- [ ] No auto-playing media

### Code Quality
- [ ] Unit tests: 100% pass (`pytest tests/`)
- [ ] Code style: No violations (`ruff check`)
- [ ] No hardcoded secrets/credentials
- [ ] Dependencies pinned in requirements.txt
- [ ] Docstrings present for functions

---

## Common Patterns

### Streamlit - Real-Time Dashboard

```python
import streamlit as st
import pandas as pd
import plotly.graph_objects as go

st.set_page_config(page_title="Batch Inference Dashboard", layout="wide")

@st.cache_data
def fetch_metrics():
    # Call backend API
    return metrics_data

metrics = fetch_metrics()

col1, col2, col3 = st.columns(3)
with col1:
    st.metric("Throughput", f"{metrics['throughput']:.0f} req/s")
with col2:
    st.metric("Latency (p99)", f"{metrics['latency_p99']:.1f}ms")
with col3:
    st.metric("Memory", f"{metrics['memory']:.1f}GB")

st.line_chart(metrics['timeseries'])
```

### Plotly Dash - Multi-Page Dashboard

```python
from dash import Dash, dcc, html
import dash_bootstrap_components as dbc

app = Dash(__name__, external_stylesheets=[dbc.themes.BOOTSTRAP])

app.layout = dbc.Container([
    dbc.Row([
        dbc.Col(html.H1("Batch Inference Monitor"), width=12)
    ]),
    dbc.Row([
        dbc.Col(dcc.Graph(id='throughput-chart'), width=6),
        dbc.Col(dcc.Graph(id='latency-chart'), width=6)
    ])
])

if __name__ == '__main__':
    app.run_server(debug=True)
```

### Gradio - Model Inference UI

```python
import gradio as gr
from transformers import AutoTokenizer, AutoModelForCausalLM

def inference(prompt):
    # Call backend or model
    output = model.generate(prompt)
    return output

gr.Interface(
    fn=inference,
    inputs=gr.Textbox(placeholder="Enter prompt..."),
    outputs=gr.Textbox(label="Output"),
    examples=["What is AI?", "Explain quantum computing"]
).launch()
```

---

## Error Handling & Recovery

### If UI Doesn't Start
1. Check Python version (3.10+)
2. Verify dependencies installed (`pip list`)
3. Check for port conflicts (Streamlit uses 8501, Dash uses 8050)
4. Review console errors for specific failures
5. Provide specific error message to Frontend-Py

### If Components Don't Render
1. Check browser console (F12) for JavaScript errors
2. Verify CSS/styling is loading
3. Check that data is flowing correctly
4. Clear browser cache and reload
5. Verify responsive design on target screen size

### If Data Doesn't Flow
1. Check API endpoint connectivity
2. Verify request/response format
3. Check for CORS issues (if applicable)
4. Review network tab in browser DevTools
5. Test API endpoint separately with `curl`/`requests`

### If Performance Is Poor
1. Profile with `streamlit run --logger.level=debug`
2. Check for N+1 queries or redundant API calls
3. Implement caching (`@st.cache_data`, `dcc.Store`)
4. Reduce dataset size for visualization
5. Implement pagination/virtual scrolling for large lists

---

## Integration with Other Subagents

### With Troels (Systems Architect)
- Troels builds Ray Data + vLLM backend systems
- Frontend-Py builds dashboards to monitor Troels' systems
- Coordinate on API contracts and data formats

### With Testing-Symbiote
- Symbiote tests backend systems
- Frontend-Py creates UI to display test results and metrics
- Share performance data and recommendations

### With Viking-Build
- Viking handles infrastructure automation
- Frontend-Py provides UIs to control/monitor Viking's infrastructure
- Coordinate on data models and event streams

---

## Deployment

### Docker Deployment

```dockerfile
FROM python:3.10-slim

WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt

COPY app.py .
EXPOSE 8501

CMD ["streamlit", "run", "app.py"]
```

### Docker Compose

```yaml
version: '3.8'
services:
  dashboard:
    build: .
    ports:
      - "8501:8501"
    environment:
      - API_URL=http://backend:8000
      - LOG_LEVEL=info
  backend:
    # Your backend service
    ports:
      - "8000:8000"
```

### Production Deployment (with Nginx)

```nginx
upstream streamlit {
    server dashboard:8501;
}

server {
    listen 80;
    server_name dashboard.example.com;

    location / {
        proxy_pass http://streamlit;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
    }
}
```

---

## Success Metrics

Frontend-Py considers a task complete when:

✓ UI starts without errors  
✓ All components render correctly  
✓ Data flows end-to-end without issues  
✓ Performance meets targets (<2s interactions)  
✓ Code is clean, modular, and well-organized  
✓ Tests pass (100% pass rate)  
✓ Documentation is clear and complete  
✓ Accessibility basics are met  
✓ Error handling is graceful  
✓ Deployment-ready (Docker/compose files included)  

---

## Support & Questions

### Getting Help
1. Provide specific task description
2. Include any relevant context or requirements
3. Mention framework preference if known
4. Share any existing UI code to build upon

### Escalation
If Frontend-Py encounters blockers:
- Requests clarification on ambiguous requirements
- Flags performance or accessibility concerns
- Documents architecture decisions
- Provides alternatives when best path unclear

---

## Configuration Reference

**Location**: `~/.config/opencode/opencode.json`

**Subagent Settings**:
- **Name**: `frontend-py`
- **Model**: Claude 3.5 Sonnet
- **Repository Scope**: sigamani/doubleword-technical
- **Mode**: Subagent (on-demand activation)
- **External Access**: Disabled (security-enforced)
- **Tools**: python, pip, git, pytest, perplexity_search, node

---

**Configuration Complete & Ready for Use**

Next: Invoke Frontend-Py with a task: `/spawn frontend-py <task>`
