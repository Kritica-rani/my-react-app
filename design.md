# CrUX Analyzer API Documentation

## 1. Design Overview

### 1.1 Current Implementation

The CrUX Analyzer API is a Node.js/Express backend service that fetches web performance metrics from Google's Chrome UX Report (CrUX) API. The current implementation:

- Accepts a list of URLs via a POST request
- Fetches multiple performance metrics for each URL in parallel using the CrUX API
- Returns consolidated results with success or error status for each URL
- Employs a mobile-first approach by using the "PHONE" form factor

### 1.2 Architecture

```
┌────────────┐     ┌─────────────────┐     ┌────────────────┐
│            │     │                 │     │                │
│ Client App │────▶│ Express Router  │────▶│ CrUX Google   │
│            │     │ /api/crux       │     │ API           │
└────────────┘     └─────────────────┘     └────────────────┘
                            │                      │
                            ▼                      │
                   ┌─────────────────┐             │
                   │ fetchCruxData() │◀────────────┘
                   │ Helper Function │
                   └─────────────────┘
```

### 1.3 API Endpoint

**POST /api/crux**

- **Request Body**:

  ```json
  {
    "urls": ["https://www.example.com", "https://www.anotherexample.com"]
  }
  ```

- **Success Response** (200 OK):

  ```json
  {
    "results": [
      {
        "url": "https://www.example.com",
        "status": "success",
        "data": {
          "record": {
            "key": {
              "url": "https://www.example.com",
              "formFactor": "PHONE"
            },
            "metrics": {
              "first_contentful_paint": { ... },
              "largest_contentful_paint": { ... },
              "cumulative_layout_shift": { ... },
              "interaction_to_next_paint": { ... },
              "experimental_time_to_first_byte": { ... }
            }
          }
        }
      },
      {
        "url": "https://www.anotherexample.com",
        "status": "error",
        "error": "No record found",
        "code": 404
      }
    ]
  }
  ```

- **Error Response** (400 Bad Request):
  ```json
  {
    "error": "'urls' should be a non-empty array."
  }
  ```

### 1.4 Metrics Collected

The API currently collects the following performance metrics:

1. **First Contentful Paint (FCP)**: Measures time from navigation to first content render
2. **Largest Contentful Paint (LCP)**: Measures loading performance (timing of largest content element)
3. **Cumulative Layout Shift (CLS)**: Measures visual stability (quantifies unexpected layout shifts)
4. **Interaction to Next Paint (INP)**: Measures responsiveness to user interactions
5. **Time to First Byte (TTFB)**: Measures time from request to first byte of response (using experimental_time_to_first_byte)

## 2. FUTURE IMPLEMENTATION

1. **No Caching Mechanism**:

   - Each request to the same URL triggers a new call to the CrUX API
   - Increased latency for repeat requests
   - Potential for hitting API rate limits

2. **No Logging Strategy**:
   - Add a error logging mechanism

## 2. Next Steps and Implementation Plan

#### 2.1.1 Implement Caching

#### 2.1.2 Implement Rate Limiting

#### 2.3.3 Implement Comprehensive Logging

## 3. Additional Documentation

### 3.1 Metric Interpretation Guide

For frontend consuming this API, here's how to interpret the metrics:

**First Contentful Paint (FCP)**

- Good: <= 1.8 seconds
- Needs Improvement: 1.8 - 3.0 seconds
- Poor: > 3.0 seconds

**Largest Contentful Paint (LCP)**

- Good: <= 2.5 seconds
- Needs Improvement: 2.5 - 4.0 seconds
- Poor: > 4.0 seconds

**Cumulative Layout Shift (CLS)**

- Good: <= 0.1
- Needs Improvement: 0.1 - 0.25
- Poor: > 0.25

**Interaction to Next Paint (INP)**

- Good: <= 200 milliseconds
- Needs Improvement: 200 - 500 milliseconds
- Poor: > 500 milliseconds

**Time to First Byte (TTFB)**

- Good: <= 800 milliseconds
- Needs Improvement: 800 - 1800 milliseconds
- Poor: > 1800 milliseconds
