Create a FastAPI backend for Oracle database comparison platform with a single-table storage approach and comprehensive API functionality.

PROJECT OVERVIEW:
Build a production-ready FastAPI backend that integrates with Arrow/DuckDB comparison engines and stores all job data (status, progress, results) in a single Oracle table for simplicity.

CORE REQUIREMENTS:

1. SINGLE TABLE STORAGE DESIGN:
- Use ONE Oracle table called COMPARISON_JOBS to store everything
- Include columns: job_id, table_name, status, progress, results (JSON), created_at, etc.
- Store detailed comparison results as JSON in the same table
- Store column mismatches, ML results, and statistics all in JSON columns
- No separate tables for results - keep it simple and unified

2. FASTAPI BACKEND STRUCTURE:
Create these main components:
- API endpoints for job management (/api/v1/compare, /api/v1/jobs, /api/v1/metrics)
- Background job processing with Celery for long-running comparisons
- WebSocket endpoints for real-time job progress updates
- Integration with existing Arrow/DuckDB comparison code
- Oracle database connection and job storage
- Authentication and error handling

3. INTEGRATION WITH ARROW/DUCKDB ENGINES:
- Import and use existing ArrowDuckDBComparison class
- Import and use existing ColumnLevelMismatchDetector class  
- Import and use existing PolarsOracleComparison class
- Import and use existing MLDataComparison class
- Coordinate all comparison types in a single workflow
- Store all results in the single COMPARISON_JOBS table as JSON

4. API ENDPOINTS TO IMPLEMENT:

A. Job Management:
POST /api/v1/compare - Submit new comparison job
GET /api/v1/jobs - List recent jobs with filtering
GET /api/v1/jobs/{job_id} - Get specific job status and summary
GET /api/v1/jobs/{job_id}/results - Get detailed comparison results
DELETE /api/v1/jobs/{job_id} - Cancel running job
PUT /api/v1/jobs/{job_id}/retry - Retry failed job

B. Dashboard Metrics:
GET /api/v1/dashboard/metrics - Get aggregated dashboard metrics
GET /api/v1/dashboard/trends - Get historical trend data
GET /api/v1/dashboard/activity - Get recent activity feed

C. System Health:
GET /api/v1/health - System health check
GET /api/v1/metrics - Prometheus metrics endpoint

D. Real-time Updates:
WebSocket /ws - Real-time job progress and system updates

5. BACKGROUND JOB PROCESSING:
- Use Celery for background job execution
- Create comparison workflow that orchestrates all comparison engines
- Update job progress in real-time during execution
- Handle job failures and retries gracefully
- Send WebSocket updates for job progress

6. DATA MODELS AND SCHEMAS:
Create Pydantic models for:
- ComparisonRequest (input validation)
- ComparisonJob (job representation)
- ComparisonResults (output format)
- DashboardMetrics (metrics summary)
- JobProgress (real-time updates)

7. ORACLE INTEGRATION:
- Create OracleJobStorage class for single-table operations
- Implement CRUD operations for COMPARISON_JOBS table
- Store all job data, results, and metadata in JSON columns
- Handle Oracle connection pooling and error recovery
- Provide efficient queries for dashboard metrics

8. TESTING STRATEGY:
- Unit tests for all API endpoints
- Integration tests with mock Oracle database
- Background job testing with Celery
- WebSocket connection testing
- Performance testing for large comparison jobs
- API documentation with OpenAPI/Swagger

9. ERROR HANDLING AND LOGGING:
- Comprehensive error handling for all API endpoints
- Structured logging for debugging and monitoring
- User-friendly error messages
- Proper HTTP status codes
- Rate limiting and request validation

10. AUTHENTICATION AND SECURITY:
- JWT token-based authentication
- Role-based access control (Admin, Operator, Viewer)
- API key authentication for service-to-service calls
- Input validation and sanitization
- CORS configuration for frontend integration

11. PERFORMANCE OPTIMIZATION:
- Connection pooling for Oracle database
- Async/await patterns for non-blocking operations
- Caching for frequently accessed data
- Background job queue management
- Database query optimization

12. REAL-TIME FEATURES:
- WebSocket connections for live job updates
- Server-sent events for dashboard metrics
- Real-time progress tracking during comparisons
- Live system health monitoring
- Instant error notifications

IMPLEMENTATION REQUIREMENTS:

1. FILE STRUCTURE:
backend/
├── app/
│   ├── main.py                    # FastAPI application entry point
│   ├── config.py                  # Configuration management
│   ├── database.py                # Oracle database connection
│   ├── api/
│   │   ├── v1/
│   │   │   ├── endpoints/
│   │   │   │   ├── jobs.py        # Job management endpoints
│   │   │   │   ├── metrics.py     # Dashboard metrics endpoints
│   │   │   │   ├── compare.py     # Comparison submission endpoint
│   │   │   │   └── websocket.py   # WebSocket endpoints
│   ├── core/
│   │   ├── comparison/            # Integration with existing Arrow/DuckDB code
│   │   ├── storage/
│   │   │   └── oracle_storage.py  # Single-table Oracle operations
│   │   ├── models/                # Pydantic models
│   │   └── schemas/               # API schemas
│   ├── workers/
│   │   ├── celery_app.py         # Celery configuration
│   │   └── comparison_worker.py   # Background job processing
│   └── tests/                     # Comprehensive test suite

2. SINGLE TABLE SCHEMA:
CREATE TABLE COMPARISON_JOBS (
    JOB_ID VARCHAR2(36) PRIMARY KEY,
    TABLE_NAME VARCHAR2(128) NOT NULL,
    STATUS VARCHAR2(20) NOT NULL,
    PROGRESS_PERCENTAGE NUMBER(5,2) DEFAULT 0,
    CREATED_AT TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    COMPLETED_AT TIMESTAMP,
    EXECUTION_TIME_SECONDS NUMBER(10,2),
    
    -- Store everything as JSON for flexibility
    REQUEST_CONFIG CLOB CHECK (REQUEST_CONFIG IS JSON),
    COMPARISON_RESULTS CLOB CHECK (COMPARISON_RESULTS IS JSON),
    COLUMN_MISMATCHES CLOB CHECK (COLUMN_MISMATCHES IS JSON),
    ML_ANALYSIS CLOB CHECK (ML_ANALYSIS IS JSON),
    ERROR_DETAILS CLOB CHECK (ERROR_DETAILS IS JSON),
    
    -- Summary fields for quick queries
    TOTAL_UAT_RECORDS NUMBER(15),
    TOTAL_PROD_RECORDS NUMBER(15),
    MATCH_PERCENTAGE NUMBER(5,2),
    ANOMALIES_DETECTED NUMBER(10),
    HAS_CRITICAL_ISSUES NUMBER(1) DEFAULT 0
);

3. BACKGROUND JOB WORKFLOW:
- Receive comparison request via API
- Store job in COMPARISON_JOBS table with PENDING status
- Queue background Celery task
- Execute comparison using existing Arrow/DuckDB engines
- Update progress in real-time via WebSocket
- Store all results in single table as JSON
- Update job status to COMPLETED

4. API RESPONSE FORMAT:
All endpoints should return consistent JSON responses:
{
  "success": true,
  "data": {...},
  "message": "Operation completed successfully",
  "timestamp": "2024-12-09T10:30:00Z"
}

5. REAL-TIME UPDATES:
WebSocket messages for job progress:
{
  "job_id": "JOB-123456",
  "status": "RUNNING",
  "progress": 67.5,
  "current_step": "Column Analysis",
  "estimated_completion": "2024-12-09T10:35:00Z"
}

6. INTEGRATION POINTS:
- Frontend React dashboard connects to these APIs
- Existing Arrow/DuckDB comparison code is imported and used
- Single Oracle table stores all data for simplicity
- WebSocket provides real-time updates to dashboard
- Background jobs handle long-running comparisons

DEVELOPMENT WORKFLOW:
1. Set up FastAPI application structure
2. Create single Oracle table for job storage
3. Implement basic CRUD operations for jobs
4. Integrate existing comparison engines
5. Add background job processing with Celery
6. Implement WebSocket real-time updates
7. Create comprehensive API endpoints
8. Add authentication and security
9. Write comprehensive tests
10. Add monitoring and logging

SUCCESS CRITERIA:
- All comparison functionality accessible via REST API
- Real-time job progress updates via WebSocket
- Single Oracle table stores all job data efficiently
- Background jobs handle long-running comparisons
- Comprehensive error handling and logging
- Full integration with existing Arrow/DuckDB code
- Production-ready performance and security
- Complete API documentation
- Comprehensive test coverage

TECHNICAL STACK:
- FastAPI for API framework
- Celery for background job processing
- WebSocket for real-time updates
- Oracle database with single table design
- Pydantic for data validation
- JWT for authentication
- Prometheus for metrics
- Structured logging for observability

The goal is to create a robust, scalable API backend that integrates seamlessly with the existing Arrow/DuckDB comparison engines while maintaining simplicity through single-table storage design.