# j-son
import json
import os

def loadSetupData():
    with open('gc_setup') as data_file:
        course = json.load(data_file)

    grades = course["course_setup"]["grade_breakdown"]
    return grades

def loadUserGradesData():
    if os.path.isfile('./grades.json') == True:
        with open('grades') as data_file:
            user_data = json.load(data_file)

        return user_data

    return {}

def askForAssignmentMarks(grades, current_grades):
    if ("mygrades" not in current_grades.keys()):
        current_grades = {"mygrades": {}}

    for key in grades:
        print("Percent for", key, "is", grades[key])
        if( key in current_grades["mygrades"].keys()):
            print("Your grade for", key, "is", current_grades["mygrades"][key])
            update = input("Do you want to change? yes/no: ")
            if (update=="no"):
                continue
        current_grades["mygrades"][key] = input("What is your Current Grade for: " + key + " . Please insert -1 if you don't have a grade yet: ")

    return current_grades

def saveGrades(current_grades):
    print (json.dumps(current_grades))
    file = open("gc_grades.json", "w")
    file.write(json.dumps(current_grades))
    file.close()

def printCurrentGrade(grades, current_grades):
    if (len(current_grades.keys()) > 0 and len(current_grades["mygrades"].keys()) > 0):
        curr_grade = 0
        for key in current_grades["mygrades"]:
            if current_grades["mygrades"][key] != -1:
                calc_grade = int(current_grades["mygrades"][key]) * grades[key] / 100
                curr_grade = curr_grade + calc_grade

        print ("Your current grade is: ", curr_grade)
    else:
        print("Welcome first time User, you need to enter your grades to calculate")

def main():
    grades_setup = loadSetupData()
    user_grades = loadUserGradesData()
    printCurrentGrade(grades_setup, user_grades)
    current_grades = askForAssignmentMarks(grades_setup, user_grades)
    saveGrades(current_grades)
    printCurrentGrade(grades_setup, current_grades)

  main()
