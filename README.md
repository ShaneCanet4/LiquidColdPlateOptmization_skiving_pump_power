# Cold Plate CFD Surrogate Optimization

This project uses CFD results from ANSYS Fluent to train a machine learning surrogate model for predicting pressure drop across different skived-fin cold plate geometries. The surrogate model is then used in an optimization loop to evaluate manufacturable cold plate designs based on fin gap, fin thickness, and number of fins.

## Project Goal

The goal of this project is to optimize a cold plate geometry for reduced pressure drop and pumping power while staying within skiving manufacturability limits.

The design variables are:

- `g_fin`: fin gap size
- `t_fin`: fin thickness
- `N_fin`: number of fins/channels

The main CFD output used for training is:

- `delta_p`: pressure drop across the cold plate

Coolant temperature rise, `delta_T`, may be used as a sanity check to confirm that the CFD case follows the expected energy balance, but it is not the main MLP output.

## Workflow

1. Create parametric cold plate geometries in SolidWorks.
2. Export each geometry as a STEP file.
3. Import each geometry into ANSYS.
4. Generate the mesh with appropriate face sizing and inflation layers.
5. Run CFD simulations in Fluent for each geometry.
6. Post-process the CFD results to extract pressure drop.
7. Manually add geometry parameters to the results CSV using Excel:
   - `N_fin`
   - `g_fin`
   - `t_fin`
8. Train the MLP surrogate model using the completed CSV file.
9. Use the trained model inside the optimization loop.

## Important CSV Requirement

Before training the MLP, the CFD results file must be post-processed and completed manually.

The required file is:

```text
cfd_results.csv
