Converting animal recordings data for simulation use
====================================================

## Find data files containing grid cells

Data files can be batch processed with scripting to identify which cell files contain grid cell firing. Grid cell plots can be previewed with [CMBHome's](https://github.com/hasselmonians/CMBHOME) Visualize Matlab script. If using data from ([Dannenburg et al., 2020](https://elifesciences.org/articles/62500)), then grid patterns of interest can be previewed in the supp. mat. plots ([link](https://cdn.elifesciences.org/articles/62500/elife-62500-fig6-data1-v2.pdf)).
<br><details>
<summary>Filenames used in the article</summary>
In the article's Fig 2., filenames used for experiments were: 
<br>191108_S1_lightVSdarkness_cells11and12.mat tetrode 1 cell 9 (T1C9) for small-grid scale.
<br>merged_sessions_ArchTChAT#22_cell1.mat T2C1 for medium-grid scale.
<br>GCaMP6fChAT10_gridCell_mergedSessions.mat T2C1 for large-grid scale.
<br>Any users wanting to use this specific data should please contact us for it.
</details>

## Converting data files into a format for the simulation

Use moves_reporter.m to convert animal movements in recording files into datasets of animal angles and speeds that can be processed by the simulation. In moves_reporter.m, set file_to_reformat to the animal data file location and set reformat_traj_data = 1. Set which tetrode and cell to use in the file with cell_selection = \[\<tetrode\>,\<cell\>\]. Select to output csv files by write_csv_file = 1. Set CMBHome_path to where [CMBHome](https://github.com/hasselmonians/CMBHOME) is located.	Run the script. The reformatted data will be output in to the output directory as angels and speeds csv files.