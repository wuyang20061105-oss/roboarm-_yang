# Robot Arm — SOLIDWORKS Redesign

SOLIDWORKS 2026 version of the BCN3D Moveo open-source robotic arm. Based on the original [Moveo](https://github.com/BCN3D/BCN3D-Moveo) design by BCN3D Technologies, redesigned from scratch in SOLIDWORKS with automated COM API tooling.

## Features

- 4 DOF + gripper: waist rotation (Y-axis), shoulder/elbow/wrist pitch (Z-axis)
- 5 printable parts + 1 assembly, single-body per part (FDM-ready)
- Joint axes use concentric cylinder faces (Φ13/Φ9 rotating joints, Φ26/Φ18 waist)
- Assembly mates: 4 concentric + 4 coincident, all mathematically defined by geometry
- Total footprint ~280mm reach, parts ~90KB each

## File Structure

```
RobotArm/
├── RobotArm.SLDASM              # Main assembly
├── RobotArm_Base.SLDPRT         # Base plate + waist cylinder
├── RobotArm_Tower.SLDPRT        # Shoulder tower + column
├── RobotArm_UpperArm.SLDPRT     # Upper arm segment
├── RobotArm_Forearm.SLDPRT      # Forearm segment
├── RobotArm_Hand.SLDPRT         # Wrist + gripper fingers
├── FIRMWARE/                    # Arduino firmware (from original Moveo)
├── USER MANUAL/                 # Assembly & usage manual
├── BOM/                         # Bill of materials
├── CAD files/                   # Original CAD files
├── STL files/                   # Print-ready STLs
├── LICENSE                      # MIT License
└── README.md                    # This file
```

## Assembly Instructions

1. Open `RobotArm.SLDASM` in SOLIDWORKS 2016 or later
2. Rotate joints by dragging individual components — mates are parametric
3. If parts are missing, ensure all `.SLDPRT` files are in the same directory

## Build from Source (COM API)

The parts were built via SOLIDWORKS COM API using C# + PowerShell. Key technical notes:

- **Template**: `gb_part.prtdot` (GB/mm), front plane = `前视基准面`
- **Extrusion**: `FeatureExtrusion3` requires `Sd=true` (both directions); single-direction returns null
- **Sketches**: one rectangle per sketch; overlapping profiles fail
- **Face selection**: `IEntity.Select2(true, 0)` with append=true; `Select2(false, 0)` clears previous
- **Mate creation**: `AddMate5` returns `Mate2` (not `Feature`); `out int ErrorStatus`
- **Surface params**: `ISurface.CylinderParams` returns `Double[7]` (origin3 + axis3 + radius); `PlaneParams` returns `Double[6]` (normal3 + point3)

## License

MIT — see [LICENSE](LICENSE)

## Credits

Original BCN3D Moveo by [BCN3D Technologies](https://github.com/BCN3D) and Departament d'Ensenyament. Based on [Andreas Hölldorfer's](http://chaozlabs.blogspot.de/) robot arm design ([Hackaday](https://hackaday.io/project/3800-3d-printable-robot-arm), [GitHub](https://github.com/4ndreas/BetaBots-Robot-Arm-Project)).
