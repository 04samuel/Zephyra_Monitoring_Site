+++
title = "LPD 2"
menu = "main"
slug = "lpd2"
+++

### Current Zephyra Parameters:
<style>
#zephyra-params table, #zephyra-params td { border: none; box-shadow: none; }
</style>
<div id="zephyra-params" style="margin-bottom:2rem;"></div>
<script>
fetch('/data/MDV_1_Current_Zephyra_Parameters.json')
  .then(r => r.json())
  .then(d => {
    const rows = [
      ['Station',                       d.station],
      ['Last Updated',                  (d.timestamp) + ' UTC'],
      ['All Event Time Adjustment',     (d.AllEventTimeAdjustmentSec / 60) + ' min'],
      ['LTS Time Adjustment',       (d.Event0TimeAdjustmentSec / 60) + ' min'],
      ['HTS Time Adjustment',       (d.Event1TimeAdjustmentSec / 60) + ' min'],
      ['LTS Duration',             (d.Event0WaitTime / 60) + ' min'],
      ['HTS Duration',             (d.Event1WaitTime / 60) + ' min'],
      ['Time Set Duration',             (d.Event2WaitTime / 60) + ' min'],
      ['Battery Stop Limit',            d.BatteryLimitStop + '%'],
      ['Battery Start Limit',           d.BatteryLimitStart + '%'],
      ['Battery Saver Switch',          d.BatteryLimitSwitch === 1 ? 'On' : 'Off'],
    ];
    const html = `
      <table style="width:100%;max-width:560px;font-size:0.9rem;">
        ${rows.map(([label, val]) => `
          <tr>
            <td style="padding:6px 12px 6px 0;font-weight:600;white-space:nowrap;opacity:0.7;">${label}</td>
            <td style="padding:6px 0;">${val}</td>
          </tr>`).join('')}
      </table>`;
    document.getElementById('zephyra-params').innerHTML = html;
  })
  .catch(() => {
    document.getElementById('zephyra-params').innerHTML = '<p style="opacity:0.5;">Could not load Zephyra parameters.</p>';
  });
</script>


### Current Lidar Parameters:
<div id="station-params" style="margin-bottom:2rem;"></div>

<style>
#station-params table, #station-params td { border: none; box-shadow: none; }
</style>

<script>
fetch('/data/MDV_1_Current_Lidar_Parameters.json')
  .then(r => r.json())
  .then(d => {
    const filename = d.Event_File.split('/').pop();
    const rows = [
      ['Station',                   d.station],
      ['Last Updated',              (d.timestamp) + ' UTC'],
      ['Timezone',                  d.Timezone],
      ['Frequency',                 d.Frequency + ' Hz'],
      ['HTS Count',                 d.HTS_num],
      ['HTS Length',        	    d.HTS_len_hours + ' hours'],
      ['HTS Total Hours',           d.HTS_total_hours + ' hours'],
      ['LTS Short Length',d.LTS_short_len_seconds + ' seconds'],
      ['LTS Long Length', d.LTS_long_len_minutes + ' minutes'],
      ['Event File',                filename],
    ];
    const html = `
      <table style="border-collapse:collapse;width:100%;max-width:560px;font-size:0.9rem;">
        ${rows.map(([label, val]) => `
          <tr>
            <td style="padding:6px 12px 6px 0;font-weight:600;white-space:nowrap;opacity:0.7;">${label}</td>
            <td style="padding:6px 0;">${val}</td>
          </tr>`).join('')}
      </table>`;
    document.getElementById('station-params').innerHTML = html;
  })
  .catch(() => {
    document.getElementById('station-params').innerHTML = '<p style="opacity:0.5;">Could not load station parameters.</p>';
  });
</script>


## LPD_2 Currently No Lidar Connected
## Testing on 6 East Roof

### 30‑Day Summary
![30 Day Graph](images/LPD_2_30_Day.svg)

### 5‑Day Summary
![5 Day Graph](images/LPD_2_5_Day.svg)


### Lidar Parameters
![30 Day Summary](images/LPD_2_Lidar_ScanParameters.svg)

### Zephyra Parameters
![5 Day Summary](images/LPD_2_Zephyra_Parameters.svg)
