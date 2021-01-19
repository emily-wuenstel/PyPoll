#Basics - import os and csv
import os
import csv 

#import data 
voter_data = os.path.join('election_data.csv')

output_file = os.path.join("voter_output.txt")

#print headers
print("Election Results")
print("----------------")

#open file so we can read the rows 
with open(voter_data) as csvfile:
    voter_csv = csv.reader(csvfile, delimiter = ',')

#total number of votes cast
    header = next(voter_csv)
    voter_count = 0

    #for each row add one to the voter count
    output = []
    if header != None:
        for row in voter_csv:
            voter_count = voter_count + 1 

            #create a list of the candidates based on unique values in the dataset (if another candidate runs, the results will self-adjust)
            if row[2] not in output:
                output.append(row[2])

    

    #print the total voter count
    print(f"Total Votes: {voter_count}")
    print("----------------")

    text_file = open(output_file, 'w')
    text_file.write("Election Results \n")
    text_file.write("---------------- \n")
    text_file.write(f"Total Votes: {voter_count} \n")
    text_file.write("---------------- \n")

#Another for loop to cycle through the list of candidates to calculate their total votes and % of total. Also keep track of the winner.
    #set the winning tracker = 0 before the for loop   
    win_votes = 0

    #for every name in the output list (all the candidates), find how many votes they got and if they got more than the current winner
    for name in output:

        #The only way I could get the row count to start over was by closing and opening the file every time. I'm sure there is a quicker way, though.
        csvfile.close()
        counter = 0 

        #re-open the file, row is now set back to 0
        with open(voter_data) as csvfile:
            voter_csv = csv.reader(csvfile, delimiter = ',')

            #now calculate vote totals 
            for row in voter_csv:
                if row[2] == name:
                    counter = counter + 1

            #calculate what percent of vote they got         
            percent_votes = "{:.0%}".format(counter / voter_count)

            #print out NAME: perc of votes (votes)
            print(f"{name}: {percent_votes} ({counter})")
            text_file.write(f"{name}: {percent_votes} ({counter}) \n")
            
            #see if they got more votes than the current winner, if yes bump them into first place
            if int(counter) > int(win_votes):
                winner = name
                win_votes = counter

print("----------------")
print(f"Winner: {winner}")

text_file.write("---------------- \n")
text_file.write(f"Winner: {winner} \n")

#Qrite output to a txt file in the same folder


text_file.close()

