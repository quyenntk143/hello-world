# Day 5 - Challenge option 1

# Option 1 Difficulty Level: Elementary: A triangle can be classified based on the lengths of its sides as equilateral, isosceles or scalene. All 3 sides of an equilateral triangle have the same length. An isosceles triangle has two sides that are the same length, and a third side that is a different length. If all of the sides have different lengths then the triangle is scalene. Write a program that reads the lengths of 3 sides of a triangle from the user. Display a message indicating the type of the triangle.

a = int(input("Length of 1st side: "))
b = int(input("Length of 2nd side: "))
c = int(input("Length of 3rd side: "))
if a==b==c:
    print("Equilateral triangle")
elif a!=b!=c:
    print("Scalene triangle")
else:
    print("Isosceles triangle")
    
