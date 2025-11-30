import csv

def merge_csv_files_any(file_list, output_file, has_header=True):
    merged_data = []
    header = None

    for file in file_list:
        with open(file, "r") as f:
            reader = csv.reader(f)
            
            # Read header if present
            if has_header:
                file_header = next(reader)
                if header is None:
                    header = file_header
                    merged_data.append(header)
            
            # Append all rows, skipping empty lines
            for row in reader:
                if row:
                    merged_data.append(row)

    # Separate header and data for sorting
    if has_header and header:
        data_rows = merged_data[1:]
    else:
        data_rows = merged_data

    # Sort by first column (assumes integer IDs)
    try:
        data_rows_sorted = sorted(data_rows, key=lambda x: int(x[0]))
    except ValueError:
        data_rows_sorted = data_rows  # If IDs not integers, skip sorting

    # Combine header back if exists
    final_data = [header] + data_rows_sorted if has_header and header else data_rows_sorted

    # Write merged CSV
    with open(output_file, "w", newline="") as out:
        writer = csv.writer(out)
        writer.writerows(final_data)

    print(f"Merged file saved as {output_file}")
    print("\nPreview of merged data:")
    for row in final_data[:5]:
        print(row)


# Example usage
file_list = ["file1.csv", "file2.csv"]  # Add your files here
output_file = "merged_output.csv"
merge_csv_files_any(file_list, output_file, has_header=True)  # set False if no headers
