# SF-Research

A template project for developing, researching, and backtesting trading signals.

## 🚀 How to Use This Template
#### To create your own repository using this template, follow these quick steps:
1. Click the Button: At the top of this repository page, click the green "Use this template" button and select "Create a new repository."
2. Configure:
   - Choose an Owner (your account).
   - Give your new repository a Name, "sf-research-{signal_name}"
3. Create: Click "Create repository from template."
4. Clone: Once your new repo is ready, clone it to your local machine:
```bash
git clone https://github.com/your-username/your-new-repo.git
```

## Environment Setup

Before running any commands, you need to configure your environment:

1. **Copy the environment template:**
```bash
cp .env.example .env
```

2. **Edit `.env` with your settings:**
```bash
# Update these values:
SIGNAL_PATH=data/signal.parquet          # Where to save your signal
WEIGHT_DIR=data/weights                  # Where backtest results go
LOG_DIR=logs                              # Where logs go

SIGNAL_NAME="Your Signal Name"            # Name your signal
GAMMA=50                                  # Risk aversion parameter
EMAIL=your-netid@byu.edu                 # Your BYU email
CONSTRAINTS=["ZeroBeta", "ZeroInvestment"]  # Portfolio constraints

# Optional: Customize SLURM settings for cluster jobs
SLURM_N_CPUS=8                           # Number of CPUs
SLURM_MEM=32G                            # Memory allocation
SLURM_TIME=03:00:00                      # Time limit
```

The `.env` file is **not** tracked in git (see `.gitignore`), so each user can have their own settings.

## Project Structure

```
sf-signal/
├── src/
│   ├── framework/
│   │   ├── ew_dash.py            # Equal-weight dashboard (do not edit)
│   │   ├── opt_dash.py           # Optimal portfolio dashboard (do not edit)
│   │   └── backtest.py       # Run the backtest (edit config only)
│   └── signal/
│       └── signal.py      # Your signal implementation (edit this)
├── data/
│   ├── signal.parquet            # Output: Your signal
│   └── weights/                  # Output: Backtest weights
└── README.md
```

## Workflow

### 1. **Implement Signal** (`signal.py`)
   - Customize date ranges, data columns, and calculation logic
   - Develop your signal logic
   - Saves signal to `data/signal.parquet`

   ```bash
   make create-signal
   ```

### 2. **View Equal-Weight Performance** (`ew_dash.py`)
   - Compare your signal against an equal-weight baseline
   - Analyze signal characteristics
   - Visualize signal properties and performance

   ```bash
   make ew-dash
   ```

### 3. **Run Backtest** (`backtest.py`)
   - Run MVO-based backtest on your signal
   - Generates optimal portfolio weights
   - Saves results to `data/weights.parquet`

   ```bash
   make backtest
   ```

### 4. **View Optimized Performance** (`opt_dash.py`)
   - View optimized portfolio performance
   - Analyze backtest returns, drawdowns, and metrics

   ```bash
   make opt-dash
   ```

## Data Files

All data files are stored in the `data/` directory:

- **`data/signal.parquet`**: Output from `signal.py`
  - Columns: `date`, `barrid`, `alpha` (your signal)
  - Format: Parquet (AlphaSchema)

- **`data/weights/*.parquet`**: Output from backtest
  - Contains: Portfolio weights and performance data
  - Format: Parquet

## Quick Start

```bash
# 1. Implement your signal
# Edit src/signal/signal.py with your logic
make create-signal

# 2. View equal-weight performance
make ew-dash

# 3. Run backtest
make backtest

# 4. View optimized performance
make opt-dash
```

## Template Files (Do Not Need to Edit)

The following files are templates and should not be modified:
- `src/framework/ew_dash.py` - Equal-weight comparison dashboard
- `src/framework/opt_dash.py` - Optimized portfolio dashboard
- `src/framework/backtest.py` - Backtest runner

If you want to edit the marimo notebooks use:
```bash
uv run marimo edit src/framework/{}_dash.py
```

**All signal customization happens in `src/signal/signal.py`.**

## Configuration

All configuration is managed through the `.env` file (copied from `.env.example`):

- **`SIGNAL_PATH`**: Where to save your generated signal (relative or absolute path)
- **`WEIGHT_DIR`**: Where backtest results will be saved
- **`LOG_DIR`**: Where backtest logs will be saved
- **`SIGNAL_NAME`**: Name for your signal
- **`GAMMA`**: Risk aversion / transaction cost parameter
- **`EMAIL`**: Your BYU email for job notifications
- **`CONSTRAINTS`**: Portfolio constraints as JSON array (e.g., `["ZeroBeta", "ZeroInvestment"]`)
- **`SLURM_N_CPUS`**: Number of CPU cores for cluster jobs
- **`SLURM_MEM`**: Memory allocation for cluster jobs
- **`SLURM_TIME`**: Time limit for cluster jobs
- **`SLURM_MAIL_TYPE`**: Email notifications (BEGIN, END, FAIL)
- **`SLURM_MAX_CONCURRENT_JOBS`**: Maximum parallel jobs

**Note:** Do not edit `src/framework/backtest.py` directly. All configuration comes from `.env`.

## Next Steps

1. Implement your signal logic in `src/signal/signal.py`
2. Run `make create-signal` to generate your signal
3. Compare against baseline with `make ew-dash`
4. Run backtest with `make backtest`
5. Analyze optimized results with `make opt-dash`
6. Iterate and refine your approach

---

**Note**: This is a template project. Customize `src/signal/signal.py` with your unique signal logic, then use the workflow above to backtest your ideas.


