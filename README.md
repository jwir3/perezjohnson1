# perezjohnson1
# https://github.com/TomEphraimPerez/perezjohnson1/new/main?readme=1
# perezjohnson1
---
title: Student Course Enrollment System-1
%% The UND enrollment process is based on (at the core):
%% https://und.edu/academics/graduate-school/academic-career-support/admitted-student-checklist.html
---
classDiagram
    note "Assume the individual has been accepted for admission or is current student"
    class EnrollmentDatabase
    EnrollmentDatabase *-- ID                      %% immutable - permanent
    EnrollmentDatabase o-- Search                  %% mutable - not permanent
    EnrollmentDatabase o-- StudentAttributes
    EnrollmentDatabase o-- InGoodStanding
    EnrollmentDatabase o-- RegisterForClasses
    EnrollmentDatabase : -String major
    EnrollmentDatabase : +bool status               %% 0, or 1. (System confidence = 0 or 1)
    EnrollmentDatabase : +bool onlineDB             %% 0, or 1. (online vs offline)
    EnrollmentDatabase : +bool isEnrolled
    class ID{
      -int NetID
      -int EMPLID
    }
    class Search{
      search()                                     %% API
    }
    class StudentAttributes{
      -bool isCurrentStudent
      -bool hasAdvisor                             %% As oppoesed to "-needAdvisor" in class RegisterForClasses                
      -int NetID_EMPLID                            %% Either will suffice
      -bool isAthlete                             
      -bool academicCareer                         %% 0 = UG, 1 = graduate
    }
    class InGoodStanding{
      -bool tuitionStatus                          %% 1 = good, 0 = see Financial Records and Notes/summary
      -int gpa                                     %% < 4 decimal places
      -bool onProbation
      -bool isDismissed                            %% true if academically dismissed, dropped out, medical-leave-of-absence, call to military duty.
      -int graduationDateExpected                  %% 6 digits. The software can flag a bool : "graduatateThis-semester" attribute
    }
    %% =  =  =  =  =  =  =  =  =  =  = Course specific  =  =  =  =  =  =  =  =  =  =  =  =
    class RegisterForClasses{
      +bool hasDegreePlanner
      -bool prerequisitesMet                    %% 1 = met, 0 = not met
      +bool holdOnAcct                          %% 0 = there is no Hold, 1 = otherwise
      +bool needsAdvisor                        %% 1 = needed, 0 = not needed. bool hasAdvisor in class StudentAttributes
      +bool submittedImmunizationDoc
      +bool hasTitleIXcertification    
      +int  lectureClassNBR                             %% 5 digits
      +int  catalogNBR                           %% 3 digits
      +int lectureSectionNBR                     %% 2 digits
      +String subjectArea                        %% E.g, Finance
      +String locationCode                       %% E.g, ONLINE ASYNCHRONOUS
      +int credits                               %% < 5 digits
      +String gradeOption                        %% E.g., A-F, P/NP
      -String enrollAction                       %% E.g, Add
      -bool waitlist                             %% 1 = yes, = no
      -bool hasDropped                           
      -int hasRepeated                           %% single digit to denote number of class retakes
      -int UND_RO_RegistrationAction             %% 1 = pending, 2 = int process, 3 = complete-rejected, 4 = approved
      -bool emptyShoppingCart                    %% 1 = yes, 0 = no
    }
    class Permission{
      -String classesMissing                  %% a list
      -bool hasInstructorPermission
      -bool hasAdvisorPermission
      -bool needsDepartmentConsent            %% False if prev, or prev-prev bools are true or Permission Notes and Summary are under review  
    } 
    RegisterForClasses o-- Permission         %% >>>>>>  Mutable (temp) "has-a / contained” <<<<<<<<<<<<<| There's no actual inheritance ( <|-- )



    
