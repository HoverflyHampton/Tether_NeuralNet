# Basic Idea
Using the tether length, tether angle, and wind predict the position of the craft relative to the ground station.

## Data needed
### Inputs
- Tether Length (From BGU.csv) (alt_TetherPosition)
- Tether Roll (From BGU.csv) (ats_IMURoll)
- Tether Pitch (From BGU.csv) (ats_IMUPitch)
- Tether Roll Offset (ats_IMURollOffset)
- Tether Pitch Offset (ats_IMUPitchOffset)
- Tether sensor tempature (ats_atstemp)
- Tether sensor health (ats_Health)
- Wind speed at craft (wnd_windSpeed)
- wind bearing at craft (wnd_windBearing)?
- wind heading at craft (wnd_windHeading)?

### Outputs
- x position of craft in meters relative to ground station
- y position of craft in meters relative to ground station
- z position of craft in meters relative to ground station

### Training data needed
- altitude of craft (from BGU.csv) (alt_baroAltitude)
- gps position of craft at launch (from pix.bin)
- gps position of craft at nearest second (from pix.bin)

### Plan for getting data
For each flight in google drive (past a data where they have new style bgu files)
- download the bgu and the pix file
- extract the pix file into a dataframe
- find the takeoff point of the craft in the pix and bgu files
    - if no takeoff point, discard the data
- find the time offset for the pix and bgu files
- save the gps lat, long and bgu baro alt at the takeoff point
- create a data file for the neural network
- for each line in the bgu files after takeoff
    - find the nearest gps value in the pix file
    - calculate the delta altitude and delta lat/lon
    - convert delta lat/lon to delta meters x and y
    - save a line in the data file with all of the inputs and the expected deltas
- save a line in the master file with the file name of the training data