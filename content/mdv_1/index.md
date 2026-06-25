+++
title = "MDV 1"
menu = "main"
slug = "mdv1"
+++

<p id="utc-clock" style="font-size:0.9em;opacity:0.6;margin-bottom:2rem;"></p>
<script>
function updateClock() {
const now = new Date();
const hh = String(now.getUTCHours()).padStart(2, '0');
const mm = String(now.getUTCMinutes()).padStart(2, '0');
document.getElementById('utc-clock').textContent = `Current Time: ${hh}:${mm} UTC`;
}
updateClock();
setInterval(updateClock, 1000);
</script>

## Located on the island of Dhigelaabadhoo, Maldives

### Zephyra Error Log (Last 30 Days):
<style>
#error-log table, #error-log td, #error-log th { border: none; box-shadow: none; }
#error-log th { text-align: left; padding-right: 2rem; opacity: 0.6; font-size: 0.85em; }
#error-log td { padding-right: 2rem; vertical-align: top; }
</style>
<div id="error-log" style="margin-bottom:2rem;"></div>
<script>
function formatTimestamp(ts) {
const parts = ts.trim().split(' ');
const dateParts = parts[0].split('-');
const time = parts[1].substring(0, 5);
return `${time} ${dateParts[2]}-${dateParts[1]}-${dateParts[0]}`;
}
fetch('/data/MDV_1_Combined_Zephyra_Log_Error.csv')
.then(r => r.text())
.then(text => {
const lines = text.trim().split('\n');
const headers = lines[0].replace(/\r/g, '').replace(/"/g, '').split(',');
const tsIdx = headers.indexOf('timestamp');
const daysIdx = headers.indexOf('days_ago');
const descIdx = headers.indexOf('description');
const rows = lines.slice(1).map(l => l.replace(/\r/g, '').replace(/"/g, '').split(','));
const filtered = rows.filter(r => parseFloat(r[daysIdx]) <= 30);
const tooMany = filtered.length > 10;
const recent = tooMany ? filtered.slice(-10) : filtered;
const display = recent.length > 0 ? recent : [rows[rows.length - 1]];
const label = recent.length > 0
? 'Errors (last 30 days)'
: 'No errors in last 30 days - showing most recent:';
let html = `<p style="margin-bottom:0.5rem;font-size:0.9em;opacity:0.7;">${label}</p>`;
html += '<table><thead><tr><th>Days Ago</th><th>Description</th><th>Timestamp</th></tr></thead><tbody>';
for (const r of display) {
html += `<tr><td>${Math.round(parseFloat(r[daysIdx]))}</td><td>${r[descIdx]}</td><td>${formatTimestamp(r[tsIdx])}</td></tr>`;
}
if (tooMany) {
html += `<tr><td colspan="3" style="opacity:0.6;font-style:italic;"> 10+ Errors, See file for additional details.</td></tr>`;
}
html += '</tbody></table>';
document.getElementById('error-log').innerHTML = html;
})
.catch(err => {
console.error('Error log fetch failed:', err);
document.getElementById('error-log').innerHTML = '<p>Error log unavailable.</p>';
});
</script>

### Profile Evolution
![10 Day Graph](images/MDV_1_10_Day_Profiles.svg)

### Profile Change
![10 Day Erosion Graph](images/MDV_1_Erosion_Accretion_Heatmap.svg)

### 30-Day Summary
![30 Day Graph](images/MDV_1_30_Day.svg)

### 5-Day Summary
![5 Day Graph](images/MDV_1_5_Day.svg)


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
['Station', d.station],
['Last Updated', (d.timestamp) + ' UTC'],
['All Event Time Adjustment', (d.AllEventTimeAdjustmentSec / 60) + ' min'],
['LTS Time Adjustment', (d.Event0TimeAdjustmentSec / 60) + ' min'],
['HTS Time Adjustment', (d.Event1TimeAdjustmentSec / 60) + ' min'],
['LTS Duration', (d.Event0WaitTime / 60) + ' min'],
['HTS Duration', (d.Event1WaitTime / 60) + ' min'],
['Time Set Duration', (d.Event2WaitTime / 60) + ' min'],
['Battery Stop Limit', d.BatteryLimitStop + '%'],
['Battery Start Limit', d.BatteryLimitStart + '%'],
['Battery Saver Switch', d.BatteryLimitSwitch === 1 ? 'On' : 'Off'],
];
const html = `<table style="width:100%;max-width:560px;font-size:0.9rem;">${rows.map(([label, val]) => `<tr><td style="padding:6px 12px 6px 0;font-weight:600;white-space:nowrap;opacity:0.7;">${label}</td><td style="padding:6px 0;">${val}</td></tr>`).join('')}</table>`;
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
['Station', d.station],
['Last Updated', (d.timestamp) + ' UTC'],
['Timezone', d.Timezone],
['Frequency', d.Frequency + ' Hz'],
['HTS Count', d.HTS_num],
['HTS Length', d.HTS_len_hours + ' hours'],
['HTS Total Hours', d.HTS_total_hours + ' hours'],
['LTS Short Length', d.LTS_short_len_seconds + ' seconds'],
['LTS Long Length', d.LTS_long_len_minutes + ' minutes'],
['Event File', filename],
];
const html = `<table style="border-collapse:collapse;width:100%;max-width:560px;font-size:0.9rem;">${rows.map(([label, val]) => `<tr><td style="padding:6px 12px 6px 0;font-weight:600;white-space:nowrap;opacity:0.7;">${label}</td><td style="padding:6px 0;">${val}</td></tr>`).join('')}</table>`;
document.getElementById('station-params').innerHTML = html;
})
.catch(() => {
document.getElementById('station-params').innerHTML = '<p style="opacity:0.5;">Could not load station parameters.</p>';
});
</script>


### Lidar Parameters
![Lidar Parameters](images/MDV_1_Lidar_ScanParameters.svg)

### Zephyra Parameters
![Zephyra Parameters](images/MDV_1_Zephyra_Parameters.svg)
