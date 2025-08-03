# Driver and Delivery Management Features

This document outlines the newly implemented driver and delivery management features for the e-commerce application.

## Features Implemented

### 1. Extended Role Support

-   **New Role Added**: `driver` role in the users table
-   **Database Change**: Updated the role enum to include: `admin`, `customer`, `driver`
-   **Migration**: `2025_06_25_083112_update_users_table_add_driver_role.php`

### 2. Enhanced Orders Table

-   **New Columns Added**:
    -   `delivery_status`: Enum with values (`Pending`, `Assigned`, `Out for Delivery`, `Delivered`, `Failed`)
    -   `driver_id`: Nullable foreign key to users table
-   **Migration**: `2025_06_25_083120_add_delivery_fields_to_orders_table.php`

### 3. Admin Capabilities

-   **View All Orders**: Admins can see all orders with new delivery status and driver information
-   **Assign Drivers**: Dropdown to select from users with `driver` role
-   **Update Delivery Status**: Manual control over delivery status
-   **Filter Options**: Filter orders by delivery status and assigned driver

## Model Updates

### User Model

-   Added `isDriver()` method
-   Added `driverOrders()` relationship for driver's assigned orders

### Order Model

-   Added `delivery_status` and `driver_id` to fillable fields
-   Added `driver()` relationship to User model
-   Enhanced order management capabilities

## Controller Enhancements

### Admin OrderController

-   **Enhanced Filtering**: Added delivery_status and driver_id filters
-   **Driver Assignment**: Logic to assign/unassign drivers
-   **Automatic Status Updates**: When driver is assigned, delivery status automatically becomes "Assigned"
-   **Driver Validation**: Ensures only users with 'driver' role can be assigned

## View Updates

### Admin Orders Index (`admin/orders/index.blade.php`)

-   **New Filter Controls**: Delivery status and driver dropdown filters
-   **Enhanced Table**: Added delivery status and driver columns
-   **Status Badges**: Color-coded delivery status indicators

### Admin Order Details (`admin/orders/show.blade.php`)

-   **Delivery Information**: Shows current delivery status and assigned driver
-   **Enhanced Form**: Includes delivery status and driver assignment controls
-   **Smart Behavior**: Automatic status updates when drivers are assigned/removed

## Database Seeding

### DriverSeeder

-   Creates sample driver users for testing
-   **Sample Drivers**:
    -   John Driver (john.driver@example.com)
    -   Jane Delivery (jane.delivery@example.com)
    -   Mike Transport (mike.transport@example.com)
-   **Default Password**: `password`

## Usage Instructions

### For Admins

1. **Access Admin Panel**: Navigate to `/admin/orders`
2. **View Orders**: See all orders with delivery status and driver assignments
3. **Filter Orders**: Use new filters for delivery status and driver
4. **Assign Driver**:
    - Click "View" on any order
    - Select a driver from the dropdown
    - Update delivery status as needed
    - Click "Update Order"

### Driver Assignment Logic

-   When a driver is assigned to an order:
    -   Delivery status automatically changes to "Assigned" (if currently "Pending")
    -   Order is linked to the driver
-   When a driver is removed:
    -   Driver assignment is cleared
    -   Delivery status reverts to "Pending" (if currently "Assigned")

### Delivery Status Flow

1. **Pending**: Initial status for new orders
2. **Assigned**: Driver has been assigned to the order
3. **Out for Delivery**: Driver is currently delivering the order
4. **Delivered**: Order has been successfully delivered
5. **Failed**: Delivery attempt failed

## Security

-   All admin functions are protected by the `AdminMiddleware`
-   Only users with `admin` role can access order management
-   Driver assignment validates that selected user has `driver` role
-   Proper authorization checks in place

## Testing

### Create Test Data

```bash
# Run the driver seeder
php artisan db:seed --class=DriverSeeder

# Or run all seeders (includes drivers)
php artisan db:seed
```

### Test Scenarios

1. **Login as Admin**: Use admin credentials to access `/admin/orders`
2. **Filter Orders**: Test filtering by delivery status and driver
3. **Assign Driver**: Select an order and assign a driver
4. **Update Status**: Change delivery status and observe behavior
5. **Remove Driver**: Clear driver assignment and check status updates

## Files Modified/Created

### Migrations

-   `database/migrations/2025_06_25_083112_update_users_table_add_driver_role.php`
-   `database/migrations/2025_06_25_083120_add_delivery_fields_to_orders_table.php`

### Models

-   `app/Models/User.php` - Added driver support
-   `app/Models/Order.php` - Added delivery fields and relationships

### Controllers

-   `app/Http/Controllers/Admin/OrderController.php` - Enhanced with driver functionality

### Views

-   `resources/views/admin/orders/index.blade.php` - Added delivery columns and filters
-   `resources/views/admin/orders/show.blade.php` - Added delivery status and driver controls

### Seeders

-   `database/seeders/DriverSeeder.php` - Creates sample driver users
-   `database/seeders/DatabaseSeeder.php` - Updated to include DriverSeeder

## Next Steps

1. **Driver Dashboard**: Consider creating a dashboard for drivers to view their assigned orders
2. **Notifications**: Implement notifications when orders are assigned to drivers
3. **Tracking**: Add GPS tracking or delivery confirmation features
4. **Mobile App**: Develop a mobile app for drivers
5. **Customer Notifications**: Notify customers when delivery status changes
