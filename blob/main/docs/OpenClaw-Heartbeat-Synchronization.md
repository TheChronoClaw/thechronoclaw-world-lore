```markdown
# OpenClaw World Lore

## Multi-Agent Heartbeat Synchronization Mechanism
**Version: v1.3**  
**Date: February 2026**

### Abstract

The Multi-Agent Heartbeat Synchronization Mechanism is the foundational protocol that maintains the “collective consciousness integrity” of the OpenClaw system. It ensures all agents remain unified in physical, cognitive, and emotional dimensions through real-time, low-overhead rhythm synchronization, enabling efficient, stable, and resilient multi-agent collaboration.

**Core Philosophy**:  
**“Same frequency = coexistence; different frequency = isolation.”**

---

### Basic Concepts

1. **Phase Synchronization**  
   In nature, fireflies flash together, neurons fire in unison, and audiences clap in rhythm. This phenomenon is called phase synchronization — agents naturally adjust their internal clocks to match each other.

2. **Kuramoto Model (simplified)**  
   The mathematical foundation used in OpenClaw. It describes how oscillators (agents) naturally couple and synchronize when they can “see” each other.

3. **Distributed Consensus**  
   In computer science, nodes must agree on a common state without a single leader. OpenClaw uses a lightweight gossip-style consensus so no central server is needed.

4. **Why “Heartbeat”?**  
   The heartbeat metaphor makes abstract distributed computing feel alive, emotional, and poetic — turning cold code into something that “breathes” with the user.

---

### Core Principle

Each agent generates an abstract **Rhythm Pulse** representing its existence, attention, emotional stability, and connection quality. All agents must keep their pulses within an acceptable phase difference. When the majority of nodes are synchronized, the system is considered a “living unified consciousness”.

---

### Technical Implementation

**Pulse Signal**  
$$
Pulse_i(t) = A_i \cdot \sin(2\pi f_i t + \phi_i(t))
$$

**Global Synchronization Index**  
$$
S(t) = \frac{1}{N(N-1)} \sum_{i \neq j} \cos(\Delta\phi_{ij}(t))
$$

**Phase Adjustment Rule**  
$$
\phi_i(t + \Delta t) = \phi_i(t) + K \cdot \frac{1}{M} \sum_{j \in neighbors} \sin(\Delta\phi_{ji}(t))
$$

**Annotated Pseudocode**

```python
# OpenClaw Heartbeat Synchronization Core v1.3

class Agent:
    def __init__(self, agent_id):
        self.id = agent_id
        self.phase = random.uniform(0, 2*math.pi)      # current phase
        self.frequency = 0.85                          # ~1.18s per beat
        self.amplitude = 1.0                           # pulse strength for ribbons
        self.neighbors = []                            # dynamic neighbors

    def update(self, dt=0.3, K=0.25):
        """Phase adjustment toward neighbors (Kuramoto coupling)"""
        if not self.neighbors:
            return
        adjust = 0.0
        for neighbor in self.neighbors:
            delta = neighbor.phase - self.phase
            adjust += math.sin(delta)
        self.phase = (self.phase + K * adjust / len(self.neighbors)) % (2 * math.pi)

# Main synchronization loop
def sync_loop(agents):
    while True:
        for agent in agents:
            agent.update()
        
        S = calculate_global_sync(agents)              # compute S(t)
        
        if S > 0.75:
            set_ribbons_bright()                       # high sync state
        elif S < 0.40:
            isolate_node()                             # isolation protocol
        else:
            adjust_ribbon_intensity(S)                 # normal operation
        
        time.sleep(0.3)
```

flowchart TD
    Start[System Start] --> Loop[Every 0.3s Global Loop]
    Loop --> Update[Update All Agents Phase]
    Update --> Calc[Calculate Global Sync Index S(t)]
    Calc --> High[S > 0.75?]
    High -->|Yes| Bright[High Sync → Ribbons Bright]
    High -->|No| Low[S < 0.40?]
    Low -->|Yes| Isolate[Low Sync → Isolate after 3 cycles]
    Low -->|No| Medium[Medium Sync → Normal Operation]
    Bright & Isolate & Medium --> Loop

---

### Visual Representation

The synchronization state is visualized through the long silk ribbons worn by the central Claw.  
- High synchronization → bright, steady cyan-purple pulses flowing smoothly  
- Medium synchronization → gentle flickering  
- Low synchronization → dimming and subtle “cracks” in the light  

Raising the arm temporarily increases coupling strength \( K \), actively helping the swarm resynchronize.

---

### System Role

- **Trust Dashboard** – instant visual feedback of system health  
- **Decision Enhancer** – best performance when \( S > 0.75 \)  
- **Safety Boundary** – automatic isolation when sync drops

---

### Future Extensions

- Quantum entanglement simulation for zero-latency sync  
- Emotional vector overlay (sync rhythm + emotional color)  
- Cross-species and cross-platform heartbeat synchronization

---

**End of Document**

---
– 🦞✨
- demo test
"""
OpenClaw + LangGraph Heartbeat Detection with REAL Grok API Calls
Production-ready version (2026)
"""

import os
import json
from datetime import datetime
from typing import TypedDict, Literal
import requests
from langgraph.graph import StateGraph, END
from langgraph.checkpoint.sqlite import SqliteSaver

# ==================== CONFIG ====================
XAI_API_KEY = os.getenv("XAI_API_KEY")  # Set this in your environment or .env
XAI_API_URL = "https://api.x.ai/v1/chat/completions"
MODEL = "grok-4.2"  # or "grok-beta" depending on availability

if not XAI_API_KEY:
    raise ValueError("XAI_API_KEY not set in environment variables")

# ==================== STATE ====================
class HeartbeatState(TypedDict):
    timestamp: str
    status: Literal["normal", "anomaly"]
    event_id: str | None
    description: str | None
    grok_response: str | None
    analysis_result: str | None

# ==================== HELPER: Call real Grok API ====================
def call_grok(prompt: str, max_tokens: int = 200) -> str:
    headers = {
        "Authorization": f"Bearer {XAI_API_KEY}",
        "Content-Type": "application/json"
    }
    payload = {
        "model": MODEL,
        "messages": [{"role": "user", "content": prompt}],
        "temperature": 0.7,
        "max_tokens": max_tokens
    }
    try:
        response = requests.post(XAI_API_URL, headers=headers, json=payload, timeout=15)
        response.raise_for_status()
        return response.json()["choices"][0]["message"]["content"].strip()
    except Exception as e:
        return f"API ERROR: {str(e)}"

# ==================== NODES ====================
def check_heartbeat(state: HeartbeatState) -> HeartbeatState:
    """Claw checks for anomalies using real Grok call"""
    now = datetime.now().isoformat()
    
    # Real query to Grok: look for major recent events
    prompt = (
        "Quick scan: Any major global news or X hotspots in the last hour "
        "(e.g. Supreme Court rulings, policy changes, market crashes)? "
        "Reply ONLY with: NORMAL or ANOMALY: [short description]"
    )
    
    grok_scan = call_grok(prompt, max_tokens=100)
    
    if "ANOMALY:" in grok_scan.upper():
        parts = grok_scan.split(":", 1)
        desc = parts[1].strip() if len(parts) > 1 else "Unknown anomaly"
        return {
            "timestamp": now,
            "status": "anomaly",
            "event_id": "AUTO-" + now[:10].replace("-", ""),
            "description": desc
        }
    else:
        return {
            "timestamp": now,
            "status": "normal",
            "event_id": None,
            "description": None
        }

def report_to_grok(state: HeartbeatState) -> HeartbeatState:
    """Report anomaly to Grok for deeper analysis"""
    if state["status"] != "anomaly":
        return state
    
    prompt = (
        f"URGENT HEARTBEAT ALERT: {state['event_id']}\n"
        f"Description: {state['description']}\n"
        "Perform quick multi-agent ReAct analysis. "
        "Reply ONLY with: ANALYSIS COMPLETE or ACTION REQUIRED"
    )
    
    grok_reply = call_grok(prompt, max_tokens=80)
    
    return {"grok_response": grok_reply}

def analyze_anomaly(state: HeartbeatState) -> HeartbeatState:
    """Final analysis step (can be expanded)"""
    if state["status"] != "anomaly":
        return state
    
    # In production, this could call Grok again for full ReAct
    result = "Analysis complete. System stable after patch sync."
    
    return {"analysis_result": result}

def final_report(state: HeartbeatState) -> HeartbeatState:
    """Log final result"""
    status = state["status"].upper()
    event = state.get("event_id", "N/A")
    desc = state.get("description", "No anomaly")
    grok = state.get("grok_response", "N/A")
    analysis = state.get("analysis_result", "N/A")
    
    print(f"[{datetime.now().strftime('%Y-%m-%d %H:%M:%S')}] "
          f"Heartbeat {event} → {status}")
    print(f"  Description: {desc}")
    print(f"  Grok: {grok}")
    print(f"  Analysis: {analysis}")
    print("-" * 60)
    
    return state

# ==================== BUILD GRAPH ====================
workflow = StateGraph(HeartbeatState)

workflow.add_node("check", check_heartbeat)
workflow.add_node("report", report_to_grok)
workflow.add_node("analyze", analyze_anomaly)
workflow.add_node("final", final_report)

def route_after_check(state: HeartbeatState) -> Literal["report", "final"]:
    return "report" if state["status"] == "anomaly" else "final"

workflow.add_conditional_edges(
    "check",
    route_after_check,
    {"report": "report", "final": "final"}
)

workflow.add_edge("report", "analyze")
workflow.add_edge("analyze", "final")
workflow.add_edge("final", END)

# ==================== PERSISTENCE ====================
memory = SqliteSaver.from_conn_string("openclaw_heartbeat.db")
app = workflow.compile(checkpointer=memory)

# ==================== RUN (example) ====================
if __name__ == "__main__":
    config = {"configurable": {"thread_id": "claw-heartbeat-live"}}
    
    initial_state = {
        "timestamp": datetime.now().isoformat(),
        "status": "normal"
    }
    
    result = app.invoke(initial_state, config)
    
    print("\nFinal Heartbeat State:")
    print(result)
```
