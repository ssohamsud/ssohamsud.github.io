# MFG Market-Making Demos

This folder contains all the interactive visualizations and dashboards for the Mean-Field Game Market-Making simulator.

## Files Overview

### Main Dashboard
- **`index.html`** - Complete interactive dashboard with all plots and metrics
- **`blog_embed_dashboard.html`** - Simplified dashboard for blog embedding

### Interactive Plots
- **`density_heatmap.html`** - Density evolution with quantile annotations
- **`control_slices.html`** - Bid/ask controls at fixed inventory levels
- **`value_function_3d.html`** - 3D value function visualization
- **`distribution_3d.html`** - 3D distribution evolution
- **`bid_control_3d.html`** - 3D bid control surface
- **`ask_control_3d.html`** - 3D ask control surface
- **`mfg_dashboard.html`** - Comprehensive dashboard with all components
- **`metrics_table.html`** - Detailed metrics table

### Static Images
- **`density_heatmap.png`** - Static density heatmap image
- **`control_slices.png`** - Static control slices image

## Usage

### For Blog Embedding
Use the embed code:
```html
<div style="text-align: center; margin: 30px 0;">
  <iframe src="/path/to/demos/index.html"
          width="100%"
          height="800"
          style="border:1px solid #ddd; border-radius:8px; box-shadow:0 4px 8px rgba(0,0,0,0.1);">
  </iframe>
  <p style="margin-top:10px; color:#666; font-size:14px;">
    Interactive Mean-Field Game Market-Making Dashboard
  </p>
</div>
```

### For Individual Plots
Each HTML file can be embedded individually:
```html
<iframe src="/path/to/demos/value_function_3d.html" width="100%" height="400"></iframe>
```

## Features

### Enhanced Metrics
- **Spread Normalization**: E[δᵃ+δᵇ] = E[ask−bid] with consistency check
- **Convergence Tracking**: Three metrics (ρ error, V error, policy change)
- **Quantile Snapshots**: Mean, 5%, 95% at t=0, T/2, T
- **Microstructure Diagnostics**: Illustrative fill rates and spread capture

### Interactive Visualizations
- **3D Surfaces**: Rotate, zoom, and explore value functions and controls
- **Heatmaps**: Distribution evolution with time markers
- **Control Slices**: Optimal strategies at fixed inventory levels
- **Convergence History**: Iterative solution process

### Reproducibility
- Fixed random seed (42)
- Environment information captured
- Complete parameter documentation

## Technical Details

### Convergence Results
- **Stop Reason**: tolerance_met
- **Iterations**: 30
- **Final Errors**: ρ=3.84e-10, V=7.96e-07, Policy=1.03e-07
- **Elapsed Time**: 0.44s

### Key Metrics
- **Full Spread**: 0.3481
- **δᵃ Mean**: 0.1741 (ask control)
- **δᵇ Mean**: 0.1741 (bid control)
- **Spread Consistency**: 0.00e+00 ✅
- **Peak Variance**: 1.1675

### Parameters
- **T**: 1.0 (final time)
- **N**: 50 (time intervals)
- **X_max**: 5.0 (max inventory)
- **M**: 50 (inventory intervals)
- **γ**: 1.0 (control cost)
- **α**: 0.1 (inventory cost)
- **β**: 0.1 (terminal cost)
- **σ**: 0.1 (volatility)

## Copy Instructions

1. Copy the entire `demos/` folder to your project
2. Update the iframe src paths in your embed code
3. Ensure all HTML files are accessible via your web server
4. The dashboard is self-contained and doesn't require external dependencies

## Notes

- All plots are interactive and responsive
- The dashboard works on desktop and mobile devices
- Microstructure diagnostics are illustrative only and don't feed back into the solver
- All visualizations use the enhanced metrics and convergence tracking from the improved simulator 