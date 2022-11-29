OpenFOAM step by step - ddy November 2022

0. copy case directory from relevant tutorial or previous work
 - 0/
 - constant/
 - system/
 - in constant/ create empty triSurface/ dir
 - in system/ make sure there are the following files: controlDict, fvSchemes, fvSolution, meshQualityDict, snappyHexMeshDict
 - touch case.foam file in case folder

1. prepare meshes in blender
 - use lower poly than final...will be easier to increse mesh resolution with snappyhexmesh than to fix misaligned vertices
 - mesh must be watertight (all edges joined to 2 faces)
 - export each component as separate stl (inlet, outlet, wall feature, etc) in ascii
 - examine each component file in a text editor - edit headers *and tailers* from 'solid Exported From Blender' to 'solid [component name]' (e.g. "solid inlet")
 - export all components in single .stl to build boundary unv in salome

2. add meshes to case folder in geometries/stl/
 - from openfoam prompt: cat geometries/stl/* > region.stl
 - check region.stl in paraview
 - check region.stl is using component names, not 'Exported from Blender'
 - move region.stl to constant/triSurface/

3. prepare boundary file in salome
 - switch to Geometry module
 - import -> .stl containing all components from blender (do not use region.stl)
 - Build Solid -> Apply & Close. If solid builds successfully the mesh is waterproof. Can delete 'solid' object
 - Inspection -> Center of Mass focused on geometry. Apply&close
 - Inspection -> Dimensions -> Bounding Box -> Apply&close
 - Select Bounding Box -> Scale An Object, select 3D scale and set Central Point to center of mass. Scale in x,y,z by 1.2. Apply&close, rename scaled object 'bounding'
 - switch to Mesh module
 - select bounding, Create Mesh. Select 3D, Algorithm -> Body Fitting. Edit Body Fitting Parameters -> Axis X/Axis Y/Axis Z -> f(t) -> 0.001 #this is essentially block resolution, closer to zero = more blocks
 - compute mesh
 - right click mesh -> Export as .unv

4. paste .unv file to geometries/ and rename bounding.unv

5. run ideasUnvToFoam geometries/bounding.unv to generate the background mesh (constant/polyMesh/)

6. open case.foam in paraview to check background mesh formation

7. edit system/snappyHexMeshDict and system/meshQualityDict for desired params

8. run snappyHexMesh

9. debug





