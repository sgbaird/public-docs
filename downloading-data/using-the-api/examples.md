---
description: API query examples with the MPRester client.
---

# Examples

## Summary Queries

### Structure data for silicon (mp-149)

```python
from mp_api import MPRester

with MPRester("your_api_key_here") as mpr:
    structure = mpr.get_structure_by_material_id("mp-149")
    
    # -- General search alternative:
    doc = mpr.summary.get_data_by_id("mp-149", fields=["structure"])
    structure = doc.structure
```

### Calculation (task) IDs for silicon (mp-149)

```python
from mp_api import MPRester

with MPRester("your_api_key_here") as mpr: 
    task_ids = mpr.get_task_ids_associated_with_material_id("mp-149")
    
    # -- General search alternative:
    doc = mpr.materials.get_data_by_id("mp-149", fields=["calc_types"])
    task_ids = doc.calc_types.keys()
    task_types = doc.calc_types.values()
```

### Band gaps for all materials containing _only_ Si and O

```python
from mp_api import MPRester

with MPRester("your_api_key_here") as mpr:
    docs = mpr.summary.search(chemsys="Si-O", 
                              fields=["material_id", "band_gap"])
    mpid_bgap_dict = {doc.material_id: doc.band_gap for doc in docs}
```

### Chemical formulas for all materials containing _at least_ Si and O

```python
from mp_api import MPRester

with MPRester("your_api_key_here") as mpr:
    docs = mpr.summary.search(elements=["Si", "O"], 
                              fields=["material_id", "band_gap"])
    mpid_formula_dict = {doc.material_id: doc.pretty_formula for doc in docs}
```

### Stable materials (on the hull) with large band gaps (>3eV)

```python
from mp_api import MPRester

with MPRester("your_api_key_here") as mpr:
    docs = mpr.summary.search(band_gap=(3,None),
                              is_stable=True, 
                              fields=["material_id"])
    stable_mpids = [doc.material_id for doc in docs]
    
    ## -- Alternative directly using energy above hull:
    docs = mpr.summary.search(band_gap=(3,None),
                              energy_above_hull=(0,0),
                              fields=["material_id"])
    stable_mpids = [doc.material_id for doc in docs]
```

## Electronic Structure

### Band structures for silicon (mp-149)

```python
from mp_api import MPRester
from emmet.core.electronic_structure import BSPathType

with MPRester("your_api_key_here") as mpr:
    # -- line-mode, Setyawan-Curtarolo (default):
    bs_sc = mpr.get_bandstructure_by_material_id("mp-149")
    
    # -- line-mode, Hinuma et al.:
    bs_hin = mpr.get_bandstructure_by_material_id("mp-149", path_type=BSPathType.hinuma)

    # -- line-mode, Latimer-Munro:
    bs_hin = mpr.get_bandstructure_by_material_id("mp-149", path_type=BSPathType.latimer_munro)
    
    # -- uniform:
    bs_uniform = mpr.get_bandstructure_by_material_id("mp-149", line_mode=False)                            
```

### Density of states for silicon (mp-149)

```python
from mp_api import MPRester

with MPRester("your_api_key_here") as mpr:
    dos = mpr.get_dos_by_material_id("mp-149")
```

## Phonons

### Band structure for silicon (mp-149)

```python
from mp_api import MPRester

with MPRester("your_api_key_here") as mpr:
    ph_bs = mpr.get_phonon_bandstructure_by_material_id("mp-149")
```

### Density of states for silicon (mp-149)

```python
from mp_api import MPRester
from emmet.core.electronic_structure import BSPathType

with MPRester("your_api_key_here") as mpr:
    ph_dos = mpr.get_phonon_dos_by_material_id("mp-149")
```

## Charge Density

### Charge density for silicon (mp-149)

```python
from mp_api import MPRester

with MPRester("your_api_key_here") as mpr:
    chgcar = mpr.get_charge_density_from_material_id("mp-149")
```
