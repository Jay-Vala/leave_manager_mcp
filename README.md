# LeaveManager MCP Server

A Model Context Protocol (MCP) server for managing employee leave requests, balances, and history. This server provides a simple and efficient way to handle leave management operations through standardized API endpoints.

## Features

- **Leave Balance Management**: Check remaining leave days for employees
- **Leave Application**: Apply for leave on specific dates
- **Leave History**: View complete leave history for employees
- **Date Range Support**: Apply for multiple consecutive or non-consecutive leave dates
- **Real-time Balance Updates**: Automatic balance updates after leave applications

## Available Functions

### 1. Get Leave Balance
Check the remaining leave days for a specific employee.

```json
{
  "function": "get_leave_balance",
  "parameters": {
    "employee_id": "string"
  }
}
```

**Response**: Returns the number of leave days remaining for the employee.

### 2. Apply Leave
Submit a leave application for specific dates.

```json
{
  "function": "apply_leave",
  "parameters": {
    "employee_id": "string",
    "leave_dates": ["2025-06-03", "2025-06-04"]
  }
}
```

**Response**: Confirms leave application and returns updated balance.

### 3. Get Leave History
Retrieve the complete leave history for an employee.

```json
{
  "function": "get_leave_history",
  "parameters": {
    "employee_id": "string"
  }
}
```

**Response**: Returns chronological list of all leave applications and their status.

## Usage Examples

### Check Leave Balance
```bash
curl -X POST http://localhost:3000/mcp \
  -H "Content-Type: application/json" \
  -d '{
    "method": "get_leave_balance",
    "params": {
      "employee_id": "E001"
    }
  }'
```

### Apply for Single Day Leave
```bash
curl -X POST http://localhost:3000/mcp \
  -H "Content-Type: application/json" \
  -d '{
    "method": "apply_leave",
    "params": {
      "employee_id": "E001",
      "leave_dates": ["2025-06-03"]
    }
  }'
```

### Apply for Multiple Days Leave
```bash
curl -X POST http://localhost:3000/mcp \
  -H "Content-Type: application/json" \
  -d '{
    "method": "apply_leave",
    "params": {
      "employee_id": "E001",
      "leave_dates": ["2025-06-03", "2025-06-04", "2025-06-05"]
    }
  }'
```

## Date Format

All dates should be provided in ISO 8601 format: `YYYY-MM-DD`

Examples:
- `2025-06-03` (June 3rd, 2025)
- `2025-12-25` (December 25th, 2025)

## Error Handling

The server provides detailed error responses for common scenarios:

- **Invalid Employee ID**: Returns error if employee doesn't exist
- **Insufficient Leave Balance**: Prevents leave application if insufficient days remaining
- **Invalid Date Format**: Returns error for malformed dates
- **Past Date Application**: Prevents application for dates in the past
- **Duplicate Leave Application**: Prevents applying for already approved dates

## API Response Format

### Success Response
```json
{
  "success": true,
  "data": {
    "message": "Leave applied for 1 day(s). Remaining balance: 17."
  }
}
```

### Error Response
```json
{
  "success": false,
  "error": {
    "code": "INSUFFICIENT_BALANCE",
    "message": "Insufficient leave balance. Required: 3, Available: 2"
  }
}
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.


## Changelog

### v1.0.0
- Initial release
- Basic leave management functionality
- Employee balance tracking
- Leave application system
- Leave history retrieval

---

**Note**: This is an MCP (Model Context Protocol) server designed to work with AI assistants and automation tools.
