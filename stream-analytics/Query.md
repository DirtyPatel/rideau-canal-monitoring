
### This Stream Analytics job reads live data from IoT Hub, groups it into 30-second windows, calculates averages and min/max values, and sends the processed results to Cosmos DB for the dashboard. At the same time, every raw message is archived into Blob Storage so nothing is lost.


WITH AggregatedData AS (
    SELECT
        location,
        System.Timestamp AS windowEnd,

        AVG(iceThickness) AS avgIceThickness,
        MAX(iceThickness) AS maxIceThickness,
        MIN(iceThickness) AS minIceThickness,

        AVG(surfaceTemp) AS avgSurfaceTemp,
        MAX(surfaceTemp) AS maxSurfaceTemp,
        MIN(surfaceTemp) AS minSurfaceTemp,

        AVG(snowAccumulation) AS avgSnow,
        MAX(snowAccumulation) AS maxSnow,
        MIN(snowAccumulation) AS minSnow,

        AVG(externalTemp) AS avgExternalTemp,
        MAX(externalTemp) AS maxExternalTemp,
        MIN(externalTemp) AS minExternalTemp

    FROM iothub
    GROUP BY
        location,
        TumblingWindow(second, 30)
)

-- 👉 Send aggregated 30-second data to Cosmos DB
SELECT
    location,
    windowEnd AS timestamp,

    avgIceThickness,
    maxIceThickness,
    minIceThickness,

    avgSurfaceTemp,
    maxSurfaceTemp,
    minSurfaceTemp,

    avgSnow,
    maxSnow,
    minSnow,

    avgExternalTemp,
    maxExternalTemp,
    minExternalTemp
INTO rideaucanaldb
FROM AggregatedData;

-- 👉 Send raw messages (unmodified) to Blob Storage for archiving
SELECT *
INTO sensorarchive
FROM iothub;
