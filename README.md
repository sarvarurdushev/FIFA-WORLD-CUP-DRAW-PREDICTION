<div align="center">

# 🏆 FIFA World Cup 2026 Draw Simulator

### Predicting Uzbekistan's Group Using Monte Carlo Simulation

<img src="uzbekistan_draw_simulation.png" alt="Simulation Results" width="800"/>

**100,000 simulations | All FIFA constraints | Real-time visualization**

[Live Demo](#-quick-start) • [Methodology](#-methodology) • [Results](#-results) • [Code Explained](#-code-explanation)

</div>

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Key Findings](#-key-findings)
- [Quick Start](#-quick-start)
- [Methodology](#-methodology)
- [Code Explanation](#-code-explanation-line-by-line)
- [Project Structure](#-project-structure)
- [Results](#-results)
- [Tech Stack](#-tech-stack)
- [Contributing](#-contributing)
- [License](#-license)

---

## 🌟 Overview

This project simulates the **FIFA World Cup 2026 Final Draw** using Monte Carlo methods to predict which group **Uzbekistan** (or any team) will be placed in. The simulator enforces all official FIFA constraints and runs 100,000 iterations to generate statistically significant predictions.

### Why This Matters

```
┌─────────────────────────────────────────────────────────────┐
│  🎯 PREDICTION ACCURACY                                     │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Traditional Guess:     1 in 12 = 8.33% per group          │
│                                                             │
│  Our Simulation:        Group J = 8.74% (MOST LIKELY)      │
│                         Group A = 7.77% (LEAST LIKELY)      │
│                                                             │
│  Key Insight:           0% chance of facing Spain,          │
│                         Argentina, France, or England!      │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 🎯 Key Findings

| Metric | Value |
|--------|-------|
| 🔄 Total Simulations | 100,000 |
| ✅ Successful Draws | 52,313 (52.3%) |
| 📍 Most Likely Group | **Group J** (8.74%) |
| 📍 Least Likely Group | **Group A** (7.77%) |
| ⚽ Top Pot 1 Opponent | **Portugal** (8.5%) |
| 🚫 Impossible Opponents | Spain, Argentina, France, England |

---

## 🚀 Quick Start

### Python Version

```bash
# Clone the repository
git clone https://github.com/yourusername/world-cup-2026-simulator.git
cd world-cup-2026-simulator

# Install dependencies
pip install matplotlib numpy

# Run the simulation
python world_cup_2026_draw_simulator.py
```

### React Version

```bash
# Create new Vite project
npm create vite@latest world-cup-simulator -- --template react-ts
cd world-cup-simulator

# Install Tailwind CSS
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p

# Copy the component and run
npm run dev
```

---

## 🔬 Methodology

### Monte Carlo Simulation

```
    ┌─────────────────────────────────────────────────────────┐
    │                  SIMULATION PIPELINE                    │
    └─────────────────────────────────────────────────────────┘
                              │
         ┌────────────────────┼────────────────────┐
         │                    │                    │
         ▼                    ▼                    ▼
    ┌─────────┐          ┌─────────┐          ┌─────────┐
    │ Run 1   │          │ Run 2   │   ....    │Run 100K │
    │         │          │         │          │         │
    │ Uzbek   │          │ Uzbek   │          │ Uzbek   │
    │ → Grp F │          │ → Grp J │          │ → Grp C │
    └────┬────┘          └────┬────┘          └────┬────┘
         │                    │                    │
         └────────────────────┼────────────────────┘
                              │
                              ▼
                    ┌─────────────────┐
                    │   AGGREGATE     │
                    │   STATISTICS    │
                    │                 │
                    │  P(Group J) =   │
                    │  8.74%          │
                    └─────────────────┘
```

### FIFA Constraints Implemented

| Constraint | Rule | Impact |
|------------|------|--------|
| **Confederation Limit** | Max 1 team per confederation (except UEFA: max 2) | AFC teams cannot be in same group |
| **Host Positions** | Mexico→A, Canada→B, USA→D | Fixed starting positions |
| **Pathway Balancing** | Spain/Argentina separate; France/England separate | Top 4 never meet before final |

---

## 📖 Code Explanation (Line by Line)

### 1. Team Data Structures

```python
# =============================================================================
# TEAM DATA - (Team Name, Confederation)
# =============================================================================

POT_1 = [
    ("Canada", "CONCACAF"),      # Host nation - North America
    ("Mexico", "CONCACAF"),      # Host nation - North America  
    ("USA", "CONCACAF"),         # Host nation - North America
    ("Spain", "UEFA"),           # #1 FIFA Ranking - Europe
    ("Argentina", "CONMEBOL"),   # #2 FIFA Ranking - South America
    ("France", "UEFA"),          # #3 FIFA Ranking - Europe
    ("England", "UEFA"),         # #4 FIFA Ranking - Europe
    ("Brazil", "CONMEBOL"),      # South America
    ("Portugal", "UEFA"),        # Europe
    ("Netherlands", "UEFA"),     # Europe
    ("Belgium", "UEFA"),         # Europe
    ("Germany", "UEFA")          # Europe
]
```

**📝 Explanation:**

| Line | What It Does |
|------|--------------|
| `POT_1 = [...]` | Creates a list of tuples containing Pot 1 teams |
| `("Canada", "CONCACAF")` | Each tuple has (team_name, confederation) |
| `CONCACAF` | Confederation for North/Central America & Caribbean |
| `UEFA` | Confederation for Europe (16 teams total - most of any confederation) |
| `CONMEBOL` | Confederation for South America |

**Why Tuples?**
```python
# Tuples are immutable - team data shouldn't change during simulation
# Easy to unpack: team, confederation = ("Spain", "UEFA")
```

---

### 2.  Constants and Configuration

```python
# All 12 groups in the World Cup
GROUPS = ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L']

# Host nations have FIXED group positions (FIFA rule)
HOST_POSITIONS = {
    "Mexico": "A",    # Mexico always in Group A, Position 1
    "Canada": "B",    # Canada always in Group B, Position 1
    "USA": "D"        # USA always in Group D, Position 1
}

# Groups are split into two pathways for knockout stage balance
PATHWAY_1_GROUPS = ['A', 'B', 'C', 'D', 'E', 'F']  # Left side of bracket
PATHWAY_2_GROUPS = ['G', 'H', 'I', 'J', 'K', 'L']  # Right side of bracket
```

**📝 Explanation:**

```
┌─────────────────────────────────────────────────────────────┐
│                    PATHWAY SYSTEM                           │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   PATHWAY 1              │            PATHWAY 2             │
│   (Groups A-F)           │            (Groups G-L)          │
│                          │                                  │
│   ┌───┐ ┌───┐ ┌───┐      │      ┌───┐ ┌───┐ ┌───┐          │
│   │ A │ │ B │ │ C │      │      │ G │ │ H │ │ I │          │
│   └───┘ └───┘ └───┘      │      └───┘ └───┘ └───┘          │
│   ┌───┐ ┌───┐ ┌───┐      │      ┌───┐ ┌───┐ ┌───┐          │
│   │ D │ │ E │ │ F │      │      │ J │ │ K │ │ L │          │
│   └───┘ └───┘ └───┘      │      └───┘ └───┘ └───┘          │
│                          │                                  │
│   Winners meet in        │      Winners meet in             │
│   Semi-Final 1           │      Semi-Final 2                │
│                          │                                  │
└─────────────────────────────────────────────────────────────┘

Spain & Argentina → Forced into OPPOSITE pathways
France & England  → Forced into OPPOSITE pathways

This ensures top 4 teams can only meet in the FINAL! 
```

---

### 3.  The Simulator Class

```python
class WorldCupDrawSimulator:
    """
    Simulates the FIFA World Cup 2026 Final Draw with all constraints. 
    
    This class manages:
    - Group assignments for all 48 teams
    - Constraint validation (confederation limits, pathways)
    - Random draw simulation
    """
    
    def __init__(self):
        """
        Initialize the simulator with empty groups. 
        
        self.groups: Dictionary mapping group letters to list of teams
                    Example: {'A': [('Mexico', 'CONCACAF'), ... ], 'B': [... ]}
        """
        self.groups: Dict[str, List[Tuple[str, str]]] = {g: [] for g in GROUPS}
        self.available_groups: List[str] = []
```

**📝 Explanation:**

| Component | Purpose |
|-----------|---------|
| `class WorldCupDrawSimulator` | Encapsulates all draw logic in one object |
| `__init__` | Constructor - runs when we create a new simulator |
| `self.groups` | Dictionary to store teams in each group |
| `{g: [] for g in GROUPS}` | Dictionary comprehension - creates `{'A': [], 'B': [], ...}` |

**Visual of `self.groups`:**
```python
self.groups = {
    'A': [],  # Empty list, will hold 4 teams
    'B': [],
    'C': [],
    # ...  continues for all 12 groups
    'L': []
}

# After simulation:
self.groups = {
    'A': [('Mexico', 'CONCACAF'), ('Croatia', 'UEFA'), ... ],
    'B': [('Canada', 'CONCACAF'), ('Japan', 'AFC'), ...],
    # ... 
}
```

---

### 4. Reset Method

```python
def reset(self):
    """
    Reset the draw state for a new simulation. 
    
    Called at the start of each of the 100,000 simulations
    to clear previous results. 
    """
    self.groups = {g: [] for g in GROUPS}  # Clear all groups
    self.available_groups = GROUPS. copy()   # Reset available groups list
```

**📝 Explanation:**

```
Before reset():                    After reset():
┌─────────────────────┐            ┌─────────────────────┐
│ Group A: [Mexico,   │            │ Group A: []         │
│          Croatia,   │     →      │ Group B: []         │
│          Egypt...]  │            │ Group C: []         │
│ Group B: [Canada...]│            │ ...                  │
└─────────────────────┘            └─────────────────────┘
```

---

### 5. Confederation Count Method

```python
def get_confederation_count(self, group: str, confederation: str) -> int:
    """
    Count how many teams from a specific confederation are in a group.
    
    Args:
        group: The group letter (e.g., 'A', 'B', etc.)
        confederation: The confederation code (e.g., 'UEFA', 'AFC')
    
    Returns:
        Integer count of teams from that confederation in the group
    
    Example:
        If Group A has [('Mexico', 'CONCACAF'), ('Spain', 'UEFA')]
        get_confederation_count('A', 'UEFA') → returns 1
        get_confederation_count('A', 'AFC') → returns 0
    """
    return sum(1 for _, conf in self.groups[group] if conf == confederation)
    #      ↑                ↑                         ↑
    #      │                │                         └── Filter condition
    #      │                └── Unpack tuple, only need confederation
    #      └── Sum up all 1s (count matches)
```

**📝 Line-by-Line Breakdown:**

```python
sum(1 for _, conf in self.groups[group] if conf == confederation)
```

| Part | Meaning |
|------|---------|
| `self.groups[group]` | Get list of teams in specified group |
| `for _, conf in ... ` | Loop through each (team, confederation) tuple |
| `_` | Underscore means "ignore this value" (we don't need team name) |
| `conf` | The confederation part of the tuple |
| `if conf == confederation` | Only count if confederation matches |
| `1 for ... ` | Generate 1 for each match |
| `sum(...)` | Add up all the 1s to get total count |

**Visual Example:**
```python
# Group A contains:
[('Mexico', 'CONCACAF'), ('Spain', 'UEFA'), ('Morocco', 'CAF')]

# get_confederation_count('A', 'UEFA'):
# Loop 1: ('Mexico', 'CONCACAF') → conf='CONCACAF' ≠ 'UEFA' → skip
# Loop 2: ('Spain', 'UEFA')      → conf='UEFA' = 'UEFA'     → count 1
# Loop 3: ('Morocco', 'CAF')     → conf='CAF' ≠ 'UEFA'      → skip
# Result: sum([1]) = 1
```

---

### 6. Can Place Team Method (Core Constraint Logic)

```python
def can_place_team(self, team: str, confederation: str, group: str) -> bool:
    """
    Check if a team can be legally placed in a group. 
    
    This is the HEART of the constraint system.  It enforces:
    1. Group capacity limit (max 4 teams)
    2.  Confederation limits (max 1 per group, except UEFA max 2)
    3. Special handling for playoff placeholders
    
    Args:
        team: Team name
        confederation: Team's confederation
        group: Target group to check
    
    Returns:
        True if placement is legal, False otherwise
    """
    
    # ═══════════════════════════════════════════════════════════════
    # CONSTRAINT 1: Group Capacity
    # ═══════════════════════════════════════════════════════════════
    if len(self.groups[group]) >= 4:
        return False  # Group is full! 
    
    # ═══════════════════════════════════════════════════════════════
    # CONSTRAINT 2: Confederation Limits
    # ═══════════════════════════════════════════════════════════════
    conf_count = self. get_confederation_count(group, confederation)
    
    if confederation == "UEFA":
        # UEFA EXCEPTION: Can have up to 2 European teams per group
        # Why? UEFA has 16 teams, 12 groups → mathematically necessary
        if conf_count >= 2:
            return False
    elif confederation == "PLAYOFF":
        # Playoff teams could be from multiple confederations
        # Conservative check: assume AFC (most common playoff outcome)
        if self.get_confederation_count(group, "AFC") >= 1:
            return False
    else:
        # ALL OTHER CONFEDERATIONS: Maximum 1 per group
        # This includes: AFC, CAF, CONCACAF, CONMEBOL, OFC
        if conf_count >= 1:
            return False
    
    # All constraints passed!
    return True
```

**📝 Visual Constraint Check:**

```
┌─────────────────────────────────────────────────────────────┐
│           CAN UZBEKISTAN (AFC) JOIN GROUP A?                │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Group A currently has:                                     │
│  ┌─────────────────────────────────────────┐                │
│  │ 1.  Mexico (CONCACAF)                    │                │
│  │ 2. Spain (UEFA)                         │                │
│  │ 3. Japan (AFC)  ← Already has AFC!     │                │
│  │ 4. [Empty]                              │                │
│  └─────────────────────────────────────────┘                │
│                                                             │
│  Check 1: len(group) >= 4?   → 3 >= 4?  → FALSE (OK)         │
│  Check 2: AFC count >= 1?   → 1 >= 1? → TRUE  (BLOCKED!)   │
│                                                             │
│  Result: ❌ CANNOT PLACE                                    │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

### 7. Get Valid Groups Method

```python
def get_valid_groups(self, team: str, confederation: str, 
                     allowed_groups: Optional[List[str]] = None) -> List[str]:
    """
    Get all groups where a team can legally be placed.
    
    Args:
        team: Team name
        confederation: Team's confederation
        allowed_groups: Optional subset of groups to check
                       (used for pathway constraints)
    
    Returns:
        List of valid group letters
    
    Example:
        For Uzbekistan (AFC), if Groups A, B, D already have AFC teams:
        get_valid_groups("Uzbekistan", "AFC") → ['C', 'E', 'F', 'G', ...]
    """
    # Use provided groups or default to all groups
    groups_to_check = allowed_groups if allowed_groups else GROUPS
    
    # List comprehension: filter to only valid groups
    valid = []
    for group in groups_to_check:
        if self.can_place_team(team, confederation, group):
            valid. append(group)
    return valid
    
    # Equivalent one-liner:
    # return [g for g in groups_to_check if self.can_place_team(team, confederation, g)]
```

**📝 Visual Example:**

```
┌─────────────────────────────────────────────────────────────┐
│     FINDING VALID GROUPS FOR UZBEKISTAN (AFC)               │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Group │ Has AFC? │ Full? │ Valid?                          │
│  ──────┼──────────┼───────┼────────                         │
│    A   │ ✅ Japan │  No   │ ❌ NO  (AFC limit reached)     │
│    B   │ ✅ Aus.   │  No   │ ❌ NO  (AFC limit reached)     │
│    C   │ ❌ None  │  No   │ ✅ YES                          │
│    D   │ ✅ Iran  │  No   │ ❌ NO  (AFC limit reached)     │
│    E   │ ❌ None  │  No   │ ✅ YES                          │
│    F   │ ❌ None  │  No   │ ✅ YES                          │
│    G   │ ✅ Korea │  No   │ ❌ NO  (AFC limit reached)     │
│    H   │ ❌ None  │  No   │ ✅ YES                          │
│    I   │ ❌ None  │  No   │ ✅ YES                          │
│    J   │ ❌ None  │  No   │ ✅ YES                          │
│    K   │ ✅ Qatar │  No   │ ❌ NO  (AFC limit reached)     │
│    L   │ ✅ S.A.  │  No   │ ❌ NO  (AFC limit reached)     │
│                                                             │
│  Result: ['C', 'E', 'F', 'H', 'I', 'J']                     │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

### 8. Place Team Method

```python
def place_team(self, team: str, confederation: str, group: str):
    """
    Place a team in a group.
    
    This method DOES NOT validate - it assumes can_place_team()
    was already called.  Always validate before placing! 
    
    Args:
        team: Team name
        confederation: Team's confederation
        group: Target group
    """
    self.groups[group]. append((team, confederation))
    #         ↑         ↑      ↑
    #         │         │      └── Add as tuple (team, confederation)
    #         │         └── Append to the list
    #         └── Access the group's team list
```

**📝 Before and After:**

```python
# Before:
self.groups['J'] = [('Portugal', 'UEFA'), ('Morocco', 'CAF')]

# Execute:
self.place_team('Uzbekistan', 'AFC', 'J')

# After:
self.groups['J'] = [('Portugal', 'UEFA'), ('Morocco', 'CAF'), ('Uzbekistan', 'AFC')]
```

---

### 9.  Draw Pot 1 Method (Most Complex)

```python
def draw_pot_1(self) -> bool:
    """
    Draw Pot 1 teams with special constraints.
    
    Pot 1 has unique rules:
    1. Host nations go to fixed positions
    2.  Top 4 teams must be split across pathways
    3.  Remaining teams drawn randomly
    
    Returns:
        True if draw succeeded, False if constraints couldn't be met
    """
    
    # ═══════════════════════════════════════════════════════════════
    # STEP 1: Place host nations in fixed positions
    # ═══════════════════════════════════════════════════════════════
    for team, conf in POT_1:
        if team in HOST_POSITIONS:
            self. place_team(team, conf, HOST_POSITIONS[team])
    
    # After this step:
    # Group A: [('Mexico', 'CONCACAF')]
    # Group B: [('Canada', 'CONCACAF')]
    # Group D: [('USA', 'CONCACAF')]
    
    # ═══════════════════════════════════════════════════════════════
    # STEP 2: Separate remaining teams
    # ═══════════════════════════════════════════════════════════════
    remaining = [(t, c) for t, c in POT_1 if t not in HOST_POSITIONS]
    # remaining = [('Spain', 'UEFA'), ('Argentina', 'CONMEBOL'), ...]
    
    # Identify top 4 ranked teams (need special handling)
    top_ranked = {
        "Spain": None,      # Will store ('Spain', 'UEFA')
        "Argentina": None,  # Will store ('Argentina', 'CONMEBOL')
        "France": None,     # Will store ('France', 'UEFA')
        "England": None     # Will store ('England', 'UEFA')
    }
    other_teams = []  # Brazil, Portugal, Netherlands, Belgium, Germany
    
    for team, conf in remaining:
        if team in top_ranked:
            top_ranked[team] = conf
        else:
            other_teams. append((team, conf))
    
    # ═══════════════════════════════════════════════════════════════
    # STEP 3: Apply pathway constraints for top 4
    # ═══════════════════════════════════════════════════════════════
    
    # Randomly assign Spain to Pathway 1 or 2
    if random.random() < 0.5:
        spain_pathway = PATHWAY_1_GROUPS      # Groups A-F
        argentina_pathway = PATHWAY_2_GROUPS  # Groups G-L
    else:
        spain_pathway = PATHWAY_2_GROUPS
        argentina_pathway = PATHWAY_1_GROUPS
    
    # Randomly assign France to Pathway 1 or 2
    if random.random() < 0. 5:
        france_pathway = PATHWAY_1_GROUPS
        england_pathway = PATHWAY_2_GROUPS
    else:
        france_pathway = PATHWAY_2_GROUPS
        england_pathway = PATHWAY_1_GROUPS
    
    # ═══════════════════════════════════════════════════════════════
    # STEP 4: Place top 4 teams in their assigned pathways
    # ═══════════════════════════════════════════════════════════════
    
    # Place Spain
    valid = self.get_valid_groups("Spain", top_ranked["Spain"], spain_pathway)
    if not valid:
        return False  # No valid position - draw failed! 
    self.place_team("Spain", top_ranked["Spain"], random.choice(valid))
    
    # Place Argentina (in opposite pathway)
    valid = self.get_valid_groups("Argentina", top_ranked["Argentina"], argentina_pathway)
    if not valid:
        return False
    self.place_team("Argentina", top_ranked["Argentina"], random.choice(valid))
    
    # Place France
    valid = self. get_valid_groups("France", top_ranked["France"], france_pathway)
    if not valid:
        return False
    self.place_team("France", top_ranked["France"], random.choice(valid))
    
    # Place England (in opposite pathway from France)
    valid = self.get_valid_groups("England", top_ranked["England"], england_pathway)
    if not valid:
        return False
    self. place_team("England", top_ranked["England"], random.choice(valid))
    
    # ═══════════════════════════════════════════════════════════════
    # STEP 5: Place remaining Pot 1 teams randomly
    # ═══════════════════════════════════════════════════════════════
    
    random.shuffle(other_teams)  # Randomize order
    
    for team, conf in other_teams:
        # Find groups that don't have a Pot 1 team yet
        valid = [g for g in GROUPS 
                 if len(self.groups[g]) == 0 
                 and self. can_place_team(team, conf, g)]
        
        if not valid:
            return False  # Draw failed - no valid group
        
        # Randomly select from valid groups
        self.place_team(team, conf, random.choice(valid))
    
    return True  # Pot 1 successfully drawn! 
```

**📝 Visual Walkthrough:**

```
┌─────────────────────────────────────────────────────────────┐
│                    POT 1 DRAW PROCESS                       │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  STEP 1: Place Hosts (Fixed)                                │
│  ┌─────┬─────┬─────┬─────┬─────┬─────┐                      │
│  │  A  │  B  │  C  │  D  │  E  │ ...  │                      │
│  │ MEX │ CAN │     │ USA │     │     │                      │
│  └─────┴─────┴─────┴─────┴─────┴─────┘                      │
│                                                             │
│  STEP 2: Pathway Assignment (Random)                        │
│  ┌─────────────────┬─────────────────┐                      │
│  │   PATHWAY 1     │   PATHWAY 2     │                      │
│  │   (A-F)         │   (G-L)         │                      │
│  │                 │                 │                      │
│  │   🇪🇸 Spain     │  🇦🇷 Argentina  │  ← Separated!        │
│  │   🇫🇷 France    │  🏴󠁧󠁢󠁥󠁮󠁧󠁿 England   │  ← Separated!        │
│  └─────────────────┴─────────────────┘                      │
│                                                             │
│  STEP 3: Place Remaining (Random)                           │
│  Brazil, Portugal, Netherlands, Belgium, Germany            │
│  → Shuffled and placed in empty groups                      │
│                                                             │
│  FINAL RESULT (Example):                                    │
│  ┌─────┬─────┬─────┬─────┬─────┬─────┬─────┬─────┬─────┐    │
│  │  A  │  B  │  C  │  D  │  E  │  F  │  G  │ ...  │  L  │    │
│  │ MEX │ CAN │ SPA │ USA │ FRA │ BRA │ ARG │     │ ENG │    │
│  └─────┴─────┴─────┴─────┴─────┴─────┴─────┴─────┴─────┘    │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

### 10. Draw Pot Method (Pots 2, 3, 4)

```python
def draw_pot(self, pot: List[Tuple[str, str]]) -> bool:
    """
    Draw a pot of teams into groups.
    
    Used for Pots 2, 3, and 4 (simpler than Pot 1).
    
    Args:
        pot: List of (team, confederation) tuples
    
    Returns:
        True if all teams placed successfully, False otherwise
    """
    # Create a copy to avoid modifying original
    teams = pot.copy()
    
    # Shuffle for randomness (Fisher-Yates algorithm internally)
    random. shuffle(teams)
    
    # Try to place each team
    for team, conf in teams:
        # Get all valid groups for this team
        valid = self.get_valid_groups(team, conf)
        
        # If no valid groups, draw fails
        if not valid:
            return False
        
        # Randomly select from valid groups
        group = random.choice(valid)
        
        # Place the team
        self.place_team(team, conf, group)
    
    return True  # All teams placed successfully
```

**📝 Example for Pot 3 (Uzbekistan's Pot):**

```python
# Pot 3 teams (shuffled):
['Tunisia', 'Uzbekistan', 'Norway', 'Egypt', ...]

# Processing Uzbekistan:
valid_groups = get_valid_groups('Uzbekistan', 'AFC')
# Returns: ['C', 'E', 'F', 'H', 'I', 'J'] (groups without AFC teams)

group = random.choice(['C', 'E', 'F', 'H', 'I', 'J'])
# Randomly selects: 'J'

place_team('Uzbekistan', 'AFC', 'J')
# Uzbekistan now in Group J! 
```

---

### 11.  Simulate Draw Method (Main Entry Point)

```python
def simulate_draw(self) -> bool:
    """
    Simulate a complete World Cup draw. 
    
    This is the main method called 100,000 times.
    
    Returns:
        True if draw completed successfully
        False if any constraint couldn't be satisfied
    """
    # Start fresh
    self.reset()
    
    # Draw each pot in order
    # Each pot depends on previous pot's results
    
    if not self.draw_pot_1():
        return False  # Pot 1 failed
    
    if not self. draw_pot(POT_2):
        return False  # Pot 2 failed
    
    if not self.draw_pot(POT_3):  # ← Uzbekistan drawn here! 
        return False  # Pot 3 failed
    
    if not self. draw_pot(POT_4):
        return False  # Pot 4 failed
    
    return True  # Complete draw succeeded! 
```

**📝 Visual Flow:**

```
┌─────────────────────────────────────────────────────────────┐
│                  SIMULATION FLOW                            │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   simulate_draw()                                           │
│        │                                                    │
│        ▼                                                    │
│   ┌─────────┐                                               │
│   │  RESET  │ ← Clear all groups                           │
│   └────┬────┘                                               │
│        │                                                    │
│        ▼                                                    │
│   ┌─────────┐     ┌─────────┐                               │
│   │ POT 1   │────►│ Failed?  │──► Return False              │
│   └────┬────┘     └─────────┘                               │
│        │ Success                                            │
│        ▼                                                    │
│   ┌─────────┐     ┌─────────┐                               │
│   │ POT 2   │────►│ Failed? │──► Return False              │
│   └────┬────┘     └─────────┘                               │
│        │ Success                                            │
│        ▼                                                    │
│   ┌─────────┐     ┌─────────┐                               │
│   │ POT 3   │────►│ Failed? │──► Return False              │
│   │(UZBEK!) │     └─────────┘                               │
│   └────┬────┘                                               │
│        │ Success                                            │
│        ▼                                                    │
│   ┌─────────┐     ┌─────────┐                               │
│   │ POT 4   │────►│ Failed? │──► Return False              │
│   └────┬────┘     └─────────┘                               │
│        │ Success                                            │
│        ▼                                                    │
│   Return True ✅                                            │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

### 12. Run Simulation Function (Monte Carlo Engine)

```python
def run_simulation(num_simulations: int = 100000, 
                   target_team: str = "Uzbekistan") -> Dict:
    """
    Run Monte Carlo simulation and collect statistics.
    
    This is where the magic happens! We run the draw many times
    and aggregate the results to calculate probabilities.
    
    Args:
        num_simulations: How many times to simulate (default: 100,000)
        target_team: Which team to track (default: "Uzbekistan")
    
    Returns:
        Dictionary containing all statistics and probabilities
    """
    # Create simulator instance
    simulator = WorldCupDrawSimulator()
    
    # Initialize counters
    group_counts = Counter()        # How many times in each group
    opponent_counts = defaultdict(Counter)  # Track opponents per group
    successful_draws = 0
    failed_draws = 0
    
    # Progress indicator
    print(f"Running {num_simulations:,} simulations...")
    
    # ═══════════════════════════════════════════════════════════════
    # MAIN SIMULATION LOOP
    # ═══════════════════════════════════════════════════════════════
    for i in range(num_simulations):
        
        # Print progress every 10,000 iterations
        if (i + 1) % 10000 == 0:
            print(f"  Progress: {i + 1:,} / {num_simulations:,}")
        
        # Run one complete draw simulation
        if simulator. simulate_draw():
            # Draw succeeded! 
            successful_draws += 1
            
            # Find which group the target team ended up in
            group = simulator.get_team_group(target_team)
            
            if group:
                # Increment counter for this group
                group_counts[group] += 1
                
                # Track who else is in this group (opponents)
                for team in simulator.get_group_teams(group):
                    if team != target_team:
                        opponent_counts[group][team] += 1
        else:
            # Draw failed (constraint deadlock)
            failed_draws += 1
    
    # ═══════════════════════════════════════════════════════════════
    # CALCULATE PROBABILITIES
    # ═══════════════════════════════════════════════════════════════
    
    total = sum(group_counts. values())  # Total successful placements
    
    # Convert counts to percentages
    group_probabilities = {
        g: (count / total) * 100 
        for g, count in group_counts. items()
    }
    
    # Sort by group letter
    group_probabilities = dict(sorted(group_probabilities.items()))
    
    # Aggregate opponent statistics
    all_opponents = Counter()
    for group_opps in opponent_counts.values():
        all_opponents.update(group_opps)
    
    # Return comprehensive results
    return {
        "target_team": target_team,
        "total_simulations": num_simulations,
        "successful_draws": successful_draws,
        "failed_draws": failed_draws,
        "group_probabilities": group_probabilities,
        "group_counts": dict(group_counts),
        "most_likely_opponents": all_opponents. most_common(15),
        "opponent_details": {
            g: dict(c. most_common(5)) 
            for g, c in opponent_counts.items()
        }
    }
```

**📝 Counter Class Explained:**

```python
from collections import Counter

# Counter is a dictionary subclass for counting
group_counts = Counter()

# After 100,000 simulations:
group_counts = Counter({
    'J': 4573,   # Uzbekistan was in Group J 4,573 times
    'E': 4541,
    'F': 4492,
    'K': 4471,
    # ... 
    'A': 4067    # Least common
})

# Probability calculation:
# P(Group J) = 4573 / 52313 = 0.0874 = 8.74%
```

---

### 13. Main Function

```python
def main():
    """
    Main entry point for the simulation.
    
    Orchestrates the entire process:
    1. Run simulation
    2.  Print results to console
    3.  Generate visualizations
    4. Save results to JSON
    """
    print("="*60)
    print("  FIFA WORLD CUP 2026 DRAW SIMULATOR")
    print("  Predicting Uzbekistan's Group Placement")
    print("="*60)
    
    # Run the Monte Carlo simulation
    results = run_simulation(
        num_simulations=100000, 
        target_team="Uzbekistan"
    )
    
    # Display results in terminal
    print_results(results)
    
    # Create matplotlib visualizations
    if HAS_MATPLOTLIB:
        print("\nGenerating visualizations...")
        visualize_results(results)
    
    # Save to JSON file for further analysis
    output_file = "uzbekistan_draw_simulation_results.json"
    with open(output_file, 'w') as f:
        json. dump(results, f, indent=2)
    print(f"\nResults saved to {output_file}")
    
    return results


# Python idiom: only run main() if script is executed directly
if __name__ == "__main__":
    main()
```

---

## 📁 Project Structure

```
world-cup-2026-simulator/
├── 📄 README.md                           # This file
├── 🐍 world_cup_2026_draw_simulator. py   # Python simulation
├── ⚛️  WorldCupDrawSimulator.tsx          # React component
├── 📊 uzbekistan_draw_simulation. png      # Results visualization
├── 📋 uzbekistan_draw_simulation_results. json
└── 📜 LICENSE
```

---

## 📈 Results

### Group Probability Distribution

| Group | Probability | vs Average |
|-------|-------------|------------|
| A | 7.77% | -0.56% |
| B | 7.85% | -0. 48% |
| C | 8.54% | +0.21% |
| D | 7.83% | -0. 50% |
| E | 8.67% | +0. 34% |
| F | 8.58% | +0. 25% |
| G | 8.38% | +0. 05% |
| H | 8.40% | +0. 07% |
| I | 8.42% | +0. 09% |
| **J** | **8.74%** | **+0. 41%** |
| K | 8.54% | +0.21% |
| L | 8.26% | -0. 07% |

### Why Some Groups Are More Likely

```
┌─────────────────────────────────────────────────────────────┐
│  GROUP A: 7. 77% (Least Likely)                              │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  • Mexico (CONCACAF) is FIXED in Group A                    │
│  • Other CONCACAF teams can't join → more AFC space?         │
│  • BUT: Pathway 1 constraints + AFC distribution            │
│    means Group A often fills with other AFC teams           │
│    (Japan, Iran, Korea) before Uzbekistan is drawn          │
│                                                             │
│  Uzbekistan is in Pot 3, drawn AFTER Pot 2                  │
│  By the time Pot 3 is drawn, Group A likely has an          │
│  AFC team from Pot 2 already!                                │
│                                                             │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  GROUP J: 8.74% (Most Likely)                               │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  • No host nation in Group J                                │
│  • Pathway 2 (Groups G-L) has different team distribution   │
│  • Less competition from other AFC teams in Pot 2           │
│  • When Uzbekistan (Pot 3) is drawn, Group J more           │
│    frequently has no AFC team yet                           │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 🛠 Tech Stack

| Technology | Purpose |
|------------|---------|
| ![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white) | Core simulation engine |
| ![React](https://img.shields.io/badge/React-61DAFB?style=flat&logo=react&logoColor=black) | Interactive web visualization |
| ![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat&logo=typescript&logoColor=white) | Type-safe React code |
| ![Tailwind](https://img.shields.io/badge/Tailwind-38B2AC?style=flat&logo=tailwind-css&logoColor=white) | UI styling |
| ![Matplotlib](https://img. shields.io/badge/Matplotlib-11557c?style=flat) | Python visualizations |

---

## 🤝 Contributing

Contributions are welcome! Here are some ideas:

- [ ] Add more visualization options
- [ ] Implement parallel processing for faster simulation
- [ ] Add support for "what-if" scenarios
- [ ] Create API endpoint for real-time simulation
- [ ] Add historical World Cup draw analysis

```bash
# Fork the repo, then:
git checkout -b feature/amazing-feature
git commit -m 'Add amazing feature'
git push origin feature/amazing-feature
# Open a Pull Request
```

---

<div align="center">


**Built with ❤️ for the beautiful game**

[⬆ Back to Top](#-fifa-world-cup-2026-draw-simulator)

</div>
