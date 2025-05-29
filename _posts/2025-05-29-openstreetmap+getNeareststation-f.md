---
title: "[CS] 가장 가까운 관측소 찾기기"
excerpt: "for each로 가장 가까운 관측소 찾는 간단한 예제 코드"

categories:
  - CS
tags:
  - [tag1, tag2]

permalink: /CS/learnOpenStreetMap/

toc: true
toc_sticky: true

date: 2025-05-29
last_modified_at: 2025-05-29
---

### 1. OpenStreetMap이란?

**OpenStreetMap(OSM)**은 누구나 자유롭게 사용할 수 있는 **오픈소스 지도 데이터 플랫폼**입니다.  
상업용 지도 API와 달리 자유롭게 데이터를 편집하거나 가공할 수 있으며, 다양한 위치 기반 서비스(LBS) 개발에 활용됩니다.
{: .notice--primary}

### 2. 가장 가까운 관측소 찾기

어떤 특정 지점(위도, 경도)에서 가장 가까운 관측소를 찾고 싶을 때, 아래와 같은 **단순 거리 기반 비교 코드**를 사용할 수 있습니다.

```js
function getNearestStation(centerLat, centerLon) {
  let minDistance = Infinity;
  let nearestStation = null;

  stationList.forEach((station) => {
    const dLat = centerLat - station.lat;
    const dLon = centerLon - station.lon;
    const distance = Math.sqrt(dLat * dLat + dLon * dLon);

    if (distance < minDistance) {
      minDistance = distance;
      nearestStation = station;
    }
  });

  return nearestStation;
}
```

- stationList: 관측소 객체 배열입니다. 각 객체는 lat, lon 속성을 가집니다.
- centerLat, centerLon: 기준이 되는 중심 좌표입니다.
- 위도/경도의 차를 계산한 후, 피타고라스 정리를 통해 두 점 사이의 유클리드 거리(Euclidean Distance)를 계산합니다.
- 가장 짧은 거리의 관측소를 반환합니다.

### 3. 주의사항

이 코드의 핵심은 단순한 직선 거리 계산입니다. 하지만 현실 세계에서는 지구가 구형이기 때문에 보다 정확한 위치 기반 계산이 필요하다면 Haversine 공식을 사용하는 것이 일반적입니다.

```js
const R = 6371; // 지구 반지름 (단위: km)
const dLat = deg2rad(lat2 - lat1);
const dLon = deg2rad(lon2 - lon1);

const a =
  Math.sin(dLat / 2) ** 2 +
  Math.cos(deg2rad(lat1)) * Math.cos(deg2rad(lat2)) * Math.sin(dLon / 2) ** 2;

const c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1 - a));
const distance = R * c; // km 단위 거리

function deg2rad(deg) {
  return deg * (Math.PI / 180);
}
```
