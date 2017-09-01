import sys
import csv

#validate string input
def getInputString(question):
	result = input(question)
	while(len(result) == 0):
		print ("Input can not be blank")
		result = input(question)
	else:
		return result

#validate integer input
def getInputInt(question):
	num = ""
	while True:
		try:
		   num = int(input(question))
		except ValueError:
		   print("Invalid input; enter a valid number")
		   continue
		else:
			if num >= 0:
				return num
			else:
				print("Number must be >= 0")
				continue

#   pseudo code for loadBook
#	books = empty list
#	load csv file with csv reader into f_csv
#	for row in f_csv
#		add row into books

def loadBook():
	books = []
	with open('books.csv') as f:
		f_csv = csv.reader(f)
		for row in f_csv:
			books.append(row)
	print (str(len(books)) + " books load from books.csv")
	return books

#add a book to book list
def addBook():
	title = getInputString("Title: ")
	author = getInputString("Author: ")
	page = getInputInt("Pages: ")
	print (title + " by " + author + ", (" + str(page) + " pages) added to reading list")
	return [title, author, page, "r"]

#go through book list, get the indexes of books with certain status
def choseBookByStatus(bookList, status):
	chosenList = []
	for index in range(len(bookList)):
		book = bookList[index]
		if book[3] == status:
			chosenList.append(index)
	return chosenList

#display books with certain status
def displayList(bookList, status):
	chosenList = choseBookByStatus(bookList, status)
	totalPage = 0
	if status == "r":
		print("Required books:")
	elif status == "c":
		print("Completed books:")

	if len(chosenList) == 0:
		print("No books");
	else:
		for n in chosenList:	
			printBook(n, bookList[n])
			totalPage += int(bookList[n][2])
		print("Total pages for " + str(len(chosenList)) + " books: " + str(totalPage))


#display one single book
def printBook(index, book):
	print (str(index) + ". " + '{: <40}'.format(book[0]) + " by " + '{: <20}'.format(book[1]) + '{: >4}'.format(book[2]) + " pages")


#   pseudo code for markBook
#	requiredList = the number list of required books
#	if requiredList is empty
#		print no required books message
#	else
#		display required list
#		input = ask for the number of choice
#		if input is beyond the size of book list
#			print no such book message
#		elif input is in requiredList
#			mark the corresponding book as completed
#			print mark completed message
#		else
#			print this book already marked message

def markBook(bookList):
	requiredList = choseBookByStatus(bookList, 'r')
	if len(requiredList) == 0:
		print("No required books")
	else:
		displayList(bookList, 'r')
		choice = getInputInt("Enter the number of a book to mark as completed\n")
		if choice >= len(bookList):
			print("There is no such book in list")
		elif choice in requiredList:
			bookList[choice][3] = 'c'
			print(bookList[choice][0] + " by " + bookList[choice][1] + "marked as completed")
		else:
			print("That book is already completed")

#write back the book list to csv and show exit message			
def exit(bookList):
	with open('books.csv','w') as f:
		f_csv = csv.writer(f)
		f_csv.writerows(bookList)
	print(str(len(bookList)) + " books saved to books.csv")
	print("Have a nice day :)")

#entry of program
def main():
	print ("Reading List 1.0 - by %s" % "Wei Siyuan")
	bookList = loadBook()
	menu = """Menu:
R - List required books 
C - List completed books
A - Add new book
M - Mark a book as completed
Q - Quit
"""

	choice = input(menu).upper()
	while (choice  != "Q"):
		if choice == "R":
			displayList(bookList, "r")
		elif choice == "C":
			displayList(bookList, "c")
		elif choice == "A":
			bookList.append(addBook())
		elif choice == "M":
			markBook(bookList)
		else:
			print("Invalid menu choice.")
		choice = input(menu).upper()
	else:
		exit(bookList)


	
	

if __name__ == "__main__":main()
© 2017 GitHub, Inc.
Terms
Privacy
Security
Status
Help
Contact GitHub
API
Training
Shop
Blog
