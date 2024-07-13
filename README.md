# LittleLemonBookingSystem
coursera
import mysql.connector
from mysql.connector import Error

# Function to connect to MySQL database
def connect_to_mysql():
    try:
        # Replace with your MySQL database details
        connection = mysql.connector.connect(
            host='localhost',
            database='little_lemon',
            user='your_username',
            password='your_password'
        )
        if connection.is_connected():
            print('Connected to MySQL database')
            return connection

    except Error as e:
        print(f"Error connecting to MySQL: {e}")
        return None

# Example function to execute a SELECT query
def select_data(connection):
    if connection:
        try:
            cursor = connection.cursor()
            cursor.execute("SELECT * FROM bookings")
            rows = cursor.fetchall()

            print("Printing all booking records")
            for row in rows:
                print(row)

        except Error as e:
            print(f"Error executing SELECT query: {e}")

        finally:
            if 'cursor' in locals():
                cursor.close()

# Example function to add a new booking
def add_booking(connection, customer_name, room_type, check_in_date, check_out_date):
    if connection:
        try:
            cursor = connection.cursor()
            query = "INSERT INTO bookings (customer_name, room_type, check_in_date, check_out_date) " \
                    "VALUES (%s, %s, %s, %s)"
            data = (customer_name, room_type, check_in_date, check_out_date)
            cursor.execute(query, data)
            connection.commit()
            print("Booking added successfully")

        except Error as e:
            print(f"Error adding booking: {e}")

        finally:
            if 'cursor' in locals():
                cursor.close()

# Example usage
def main():
    connection = connect_to_mysql()

    # Example SELECT operation
    select_data(connection)

    # Example INSERT operation
    add_booking(connection, "John Doe", "Double", "2024-07-15", "2024-07-20")

    if connection:
        connection.close()
        print("MySQL connection closed")

if __name__ == "__main__":
    main()
