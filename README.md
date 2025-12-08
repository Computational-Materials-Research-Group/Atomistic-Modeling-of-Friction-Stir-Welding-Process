# Atomistic Modeling of Friction Stir Welding Process

A molecular dynamics simulation of the Friction Stir Welding process for joining two aluminum workpieces using LAMMPS (Large-scale Atomic/Molecular Massively Parallel Simulator).

<img width="1600" height="1200" alt="fsw1" src="https://github.com/user-attachments/assets/25b9ce0c-d7f1-41d9-8e24-869e6e997d5d" />


## Overview

This simulation models the FSW process at the atomic scale, capturing the essential physics of tool rotation, plunge, traverse, and retraction phases. The simulation includes heat generation from friction, material flow, and microstructural evolution during welding.

## System Description

### Workpieces
- **Material**: Aluminum (FCC structure, lattice constant: 4.05 Å)
- **Configuration**: Two plates in butt joint configuration
  - Left plate: 149 Å × 200 Å × 10 Å
  - Right plate: 150 Å × 200 Å × 10 Å
  - Gap: 1 Å between plates
- **Total dimensions**: 300 Å × 200 Å × 10 Å

### FSW Tool
- **Material**: Tungsten (BCC structure, lattice constant: 3.16 Å)
- **Geometry**:
  - Shoulder diameter: 24 Å (radius: 12 Å)
  - Shoulder thickness: 4 Å
  - Pin diameter: 7 Å (radius: 3.5 Å)
  - Pin length: 9 Å (90% penetration)
- **Initial position**: Centered on gap (x = 149.5 Å, y = 5 Å)

## Simulation Features

### Process Phases

1. **Setup & Equilibration**
   - System initialization at 300 K
   - Energy minimization to remove atomic overlaps
   - Thermal equilibration

2. **Tool Plunge**
   - Rotation: 500 timestep period (~4000 RPM)
   - Plunge rate: 0.004 Å/timestep
   - Temperature ramp: 300 K → 600 K
   - Duration: 50,000 timesteps

3. **Traverse (Welding)**
   - Travel direction: +Y (along joint line)
   - Traverse speed: 0.04 Å/timestep
   - Distance: 190 Å (from y=5 to y=195)
   - Duration: 4,750,000 timesteps
   - Maintained temperature: 600 K

4. **Tool Retraction**
   - Retraction rate: 0.015 Å/timestep
   - Duration: 100,000 timesteps

5. **Cooling**
   - Gradual cooling: 600 K → 300 K
   - Final equilibration at 300 K

### Analysis Capabilities

The simulation outputs several analysis metrics:

- **Temperature profiles**: Along X and Y directions
- **Density distribution**: Across the weld zone
- **Coordination number**: Atomic bonding analysis
- **Centro-symmetry parameter**: Defect and grain structure detection
- **Trajectory files**: For visualization of material flow

## Requirements

### Software
- LAMMPS (with Metal units support)
- Visualization tools: OVITO, VMD, or similar

### Potential File
- `CuAlW.txt` - EAM/alloy potential file for Al-W system
  - Must be in LAMMPS working directory
  - Available from NIST Interatomic Potentials Repository

## Usage

### Running the Simulation

```bash
lammps -in fsw_simulation.in
```

### Output Files

**Trajectory Files:**
- `initial_two_workpieces.xyz` - Initial configuration
- `system_with_tool.xyz` - System after tool insertion
- `system_after_overlap.xyz` - After overlap removal
- `system_minimized.xyz` - After energy minimization
- `system_equilibrated.xyz` - After thermal equilibration
- `fsw_process.lammpstrj` - Full FSW process trajectory
- `fsw_after_plunge.xyz` - After plunge phase
- `fsw_after_traverse.xyz` - After traverse phase
- `fsw_after_retract.xyz` - After retraction
- `fsw_cooled.xyz` - After cooling
- `fsw_final_structure.xyz` - Final structure

**Analysis Files:**
- `weld_zone_final.lammpstrj` - Detailed weld zone data
- `weld_temperature_profile.dat` - Temperature distribution (X-direction)
- `weld_temperature_profile_y.dat` - Temperature distribution (Y-direction)
- `weld_density_profile.dat` - Density distribution
- `fsw_final.restart` - LAMMPS restart file
- `fsw_final.data` - Final atomic configuration

## Visualization

### Using OVITO

```bash
ovito fsw_process.lammpstrj
```

Recommended color schemes:
- **By atom type**: Aluminum (blue), Tungsten (red)
- **By centro-symmetry**: Visualize defects and grain boundaries
- **By coordination number**: Identify bonding changes

### Using VMD

```bash
vmd -e fsw_process.lammpstrj
```

## Simulation Parameters

| Parameter | Value | Unit |
|-----------|-------|------|
| Timestep | 0.0005 | ps |
| Initial Temperature | 300 | K |
| Peak Temperature | 600 | K |
| Tool Rotation Period | 500 | timesteps |
| Boundary Conditions | Periodic (x,y), Shrink-wrapped (z) | - |
| Neighbor List Cutoff | 2.0 | Å |

## Physical Insights

This simulation can reveal:

1. **Material Flow Patterns**: Visualization of stirring zone evolution
2. **Heat Affected Zone (HAZ)**: Temperature gradients around the tool
3. **Grain Structure**: Through centro-symmetry analysis
4. **Defect Formation**: Voids, incomplete bonding at the interface
5. **Process Parameters**: Effect of rotation speed and traverse rate

## Customization

### Adjusting Process Parameters

```lammps
# Rotation speed
variable rot_period equal 500  # Decrease for faster rotation

# Traverse speed
variable traverse_speed equal 0.04  # Increase for faster welding

# Peak temperature
fix 4 thermostat nvt temp 300.0 600.0 0.1  # Modify target temperature
```

### Modifying Geometry

```lammps
# Tool dimensions
region tool_shoulder cylinder z ${tool_x} ${tool_start_y} 12.0 10 14
region tool_pin cylinder z ${tool_x} ${tool_start_y} 3.5 1 10

# Workpiece dimensions
region box block 0 300 0 200 0 50
```

## Known Limitations

- Atomic scale simulation (nanoscale vs. real macroscale FSW)
- Short simulation time (nanoseconds vs. real seconds)
- Simplified tool geometry (no threads on pin)
- No backing plate interaction
- Fixed boundary conditions may affect edge effects

## References

1. LAMMPS Documentation: https://docs.lammps.org
2. FSW Process: https://en.wikipedia.org/wiki/Friction_stir_welding
3. EAM Potentials: https://www.ctcms.nist.gov/potentials/

## Contributing

Contributions are welcome! Please feel free to submit issues or pull requests for:
- Bug fixes
- Performance improvements
- Additional analysis features
- Documentation enhancements

## License

This simulation script is provided for educational and research purposes.

## Citation

If you use this simulation in your research, please cite:

```
[Akshansh Mishra]
Atomistic Modeling of Friction-Stir Welding-Process
GitHub Repository: [https://github.com/akshansh11]
Year: 2025
```

## Contact

For questions or collaborations, please open an issue on GitHub.

---

**Note**: This is a simplified educational simulation. Real FSW processes involve much larger scales and additional physical phenomena not captured at the atomic level.
