# C++ code
//============================================================================
// Name        : Hotel Reservation C++
// Author      : Ba Nam Nguyen
// Version     :
// Copyright   : 
// Description : Hotel reservation
//============================================================================

#include <iostream>
using namespace std;
#include <iomanip>
  
 
class Date {

public:
	Date();
	Date(int, int, int);

	int get_day();
	int get_month();
	int get_year();

	void set_day(int);
	void set_month(int);
	void set_year(int);
	void print_date();

private:
	int day;
	int month;
	int year;
};

class Guests{

public:
	Guests();
	int get_check_in_day();
	int get_check_out_day();
	Information *get_guest_info(int);
	void set_guest_info(Information *, int);
	void set_checkin(int); //day in March 2022
	void set_checkout(int);// day in March 2022
	void print_checkin_checkout();

private:
	Information *guest_info[4];
	Date check_in;
	Date check_out;
	int room; //from 1 to 20

};
 
  
class Information{

public:
	Information();
	Information(char [], char[], Date);
	void print_name();
	void set_dob(int, int, int);
	void print_dob();

private:
	char first_name[20];
	char last_name[20];
	Date date_of_birth;
};

class Reservation_Manager{

public:
	Reservation_Manager();
	bool process_reserv_request(Guests_Res_Request*);
	Guests_Res_Request* get_arr(int);
	bool delete_reserv_request(Guests_Res_Request*);
	bool print_all_data(Guests_Res_Request*); //for testing
	int get_room_nights(int, int);
	void reset_room_nights(int, int);



private:
	int max_no_of_nights; //only 7 allowed
	int available_rooms; //only has 20 rooms
	Guests_Res_Request *arr[140]; // 7 times 20 = 140 if all single days are booked
	int room_nights[7][20];
};

class Guests_Res_Request{

public:
	static int last_res_id; //initial = 0
	Guests_Res_Request();
	bool build_res_request();
	int get_res_id();
	void set_guests(Guests *g);
	Guests* get_guests();

private:
	Guests *guests;
	int res_id;
	int nights;

};

  

Date::Date()
{
	day = month = year = 0;
}

Date::Date (int d, int m, int y)
{
	day = d;
	month = m;
	year = y;
}

int Date::get_day()
{
	return day;
}

int Date::get_month()
{
	return month;
}

int Date::get_year()
{
	return year;
}

void Date::set_day(int d)
{
	day = d;
}

void Date::set_month(int m)
{
	month = m;
}

void Date::set_year(int y)
{
	year = y;
}

void Date::print_date()
{
	cout << year << "-" << month << "-" << day;
}

Information::Information(){

	int day, month, year;

	Date d = Date(0,0,0);
	date_of_birth = d;

	for (int i = 0; i < 20;i++){
		first_name[i] = ' ';
		last_name[i] = ' ';
	}
	//collect guest info

	cout << "Enter first name: ";
	cin >> first_name;

	cout << "Enter last name: ";
	cin >> last_name;

	do{
		cout << "Enter birthday  (DD MM YYYY): ";
		cin >> day >> month >> year;
	} while (year > 2022 || month < 1 || month > 12 || day < 1 || day > 31); // if info is wrong, code will loop back

	set_dob(day, month, year);
}

void Information::set_dob(int d, int m, int y)
{
	date_of_birth.set_day(d);
	date_of_birth.set_month(m);
	date_of_birth.set_year(y);
}

Information::Information(char f[], char l[], Date d)
{
	for (int i = 0; i < 20; i++)
	{
		first_name[i] = f[i];
		last_name[i] = l[i];
	}
	set_dob(d.get_day(), d.get_month(), d.get_year());
}

void Information::print_name(){
	for (int i=0; i<10; i++)
		cout << first_name[i];
	cout << " ";

	for (int i=0; i<10; i++)
		cout << last_name[i];
}

void Information::print_dob(){
	date_of_birth.print_date();
}

Guests::Guests(){
	Date dt;
	dt.set_day(0);
	dt.set_month(3);
	dt.set_year(2022);
	check_in = check_out = dt;
	room = 0;
	for (int i = 0; i<4; i++) guest_info[i] = NULL;
}

int Guests::get_check_in_day(){
	return check_in.get_day();
}

int Guests::get_check_out_day(){
	return check_out.get_day();
}

Guests* Guests_Res_Request::get_guests(){
	return guests;
}

Information *Guests::get_guest_info(int i){
	return guest_info[i];
}

void Guests::set_guest_info(Information* inf, int i){
	guest_info[i] = inf;
}

void Guests::set_checkin(int d){
	Date dt;
	dt.set_day(d);
	dt.set_month(3);
	dt.set_year(2022);
	check_in = dt;
}

void Guests::set_checkout(int d){
	Date dt;
	dt.set_day(d);
	dt.set_month(3);
	dt.set_year(2022);
	check_out = dt;
}

int Guests_Res_Request::last_res_id = 0;

Guests_Res_Request::Guests_Res_Request(){
	//res_id = ++last_res_id;
	last_res_id = last_res_id + 1;
	res_id = last_res_id;
	nights = 0;
	guests = new Guests();
}

int Guests_Res_Request::get_res_id(){
	return res_id;
}

void Guests_Res_Request::set_guests(Guests *g){
	guests = g;
}

void Guests::print_checkin_checkout(){
	check_in.print_date();
	cout << " ";
	check_out.print_date();
}

bool Guests_Res_Request::build_res_request(){

	int num;
	do{
		cout << "Enter number of guests in the room: ";
		cin >> num;
	} while (num < 1||num>4);

	Guests *g = new Guests();

	for (int i=0; i<num; i++){
		cout << "Client # " << i+1 << endl;
		Information *info = new Information();
		//let constructor collect info

		g->set_guest_info(info, i);
	}

	Date d;
	d.set_month(03); //March
	d.set_year(2022); // current year

	int checkin = 0, checkout = 0;

	do{
		do{
			cout << "Enter Check-in day in March: ";
			cin >> checkin;
		} while (checkin < 1|| checkin >7);

		g->set_checkin(checkin);

		do {
			cout << "Enter check-out day in March: ";
			cin >> checkout;
		} while (checkout <= checkin || checkout >8);

		g->set_checkout(checkout);
	} while (checkin > checkout);

	set_guests(g);

	return true;
}

Reservation_Manager::Reservation_Manager(){

	max_no_of_nights = 7;
	available_rooms = 20;
	for (int i = 0; i<140; i++) arr[i] = NULL;

	for (int day = 0; day < 7; day++)
		for (int room = 0; room <20; room++)//rooms
			room_nights[day][room] = 0; // all rooms available
}

Guests_Res_Request *Reservation_Manager::get_arr(int i){
	return arr[i];
}

int Reservation_Manager::get_room_nights(int i, int j){
	return room_nights[i][j];
}

bool Reservation_Manager::delete_reserv_request(Guests_Res_Request *rq){
	//delete all
	Guests *g = rq ->get_guests();
	for (int i = 0; i<4; i++){
		Information *inf = g->get_guest_info(i);
		delete inf;
	}

	delete g; //for guests
	delete rq; //for reservation req
	return true;
}

bool Reservation_Manager::print_all_data(Guests_Res_Request* rq){
	Guests* g = rq->get_guests();
	for (int i = 0; i < 4; i++){
		Information* inf = g->get_guest_info(i);
		if (inf != NULL){
			cout << "guest " << (i+1) << ", Name: ";
			inf ->print_name();
			cout << " Birthday in YYYY MM DD: ";
			inf ->print_dob();
			cout << endl;
		}
	}
	cout << "Check-in/out (YYYY MM DD): ";
	g->print_checkin_checkout();
	cout << endl;
	return true;
}

bool Reservation_Manager::process_reserv_request( Guests_Res_Request *rrp){

	//verify available rooms

	Guests* g = rrp->get_guests();
	int start_day = g->get_check_in_day();
	int end_day = g->get_check_out_day();


	int room_found = 30; //30 = room not found because only have 20 rooms
	for (int room=0; room<20 && room_found == 30; room++) {
		for (int day = start_day - 1; day < 7; day++) {
			if (room_nights[day][room] !=0) {
				continue; //room not available. to next
			}
			else {
				if (day == end_day - 2){
					room_found = room + 1; //room index to room #
					break;
				}
				else{
					cout << "night = " << (day + 1) << ", room = " << (room + 1) << endl;
				}
			}
		}
		cout << "Room "<< room << " not available." << endl;
	}


	if (room_found == 50){
		cout << "No rooms available for these days." << endl;

		(rrp->last_res_id)--; //id not confirmed

		cout << "All data deleted (exept reservation manager data)." << endl;
		delete_reserv_request(rrp);
		return false;

	}

	//I am printing the data to show the list
	cout << "Available room: " << room_found << endl;
	cout << "Reservation ID: " << rrp->get_res_id() << endl;

	//update ID is list
	for (int day = start_day - 1; day < end_day - 1; day++)
	{
		cout << "Night of day reserved: " << (day + 1) << endl;
		room_nights[day][room_found - 1] = rrp->get_res_id();
	}

	//add to array
	arr[rrp->get_res_id()- 1] = rrp; // ID is relative to 1

	return true;
}

void Reservation_Manager::reset_room_nights(int i, int j){
	room_nights[i][j] = 0;
}


int main() {
	Reservation_Manager* rmanager = new Reservation_Manager();
	int res_ID = 0;
	int option = 1;

	while (option != 0) {
		cout << endl << endl;
		cout<< "Welcome to C++ Hotel." << endl;
		cout << endl <<"1. Start reservation. " << endl << "2. Delete reservation. " << endl << "3. View list. " << "0. Quit" << endl;
		cout << "===> ";
		cin >> option;

		if (option == 1){
			Guests_Res_Request *rrp = new Guests_Res_Request();
			rrp->build_res_request();
			rmanager->process_reserv_request(rrp);
			rmanager->print_all_data(rrp); //this is to print table

		}
		else if (option == 2){
			cout << "Please enter your reservation ID: ";
			cin >> res_ID;
			Guests_Res_Request *rr = rmanager->get_arr(res_ID);
			if (rr!= NULL)
				rmanager ->delete_reserv_request(rr);
			for (int i=0; i<7; i++){
				for (int j=0; j<20; j++){
					if (rmanager->get_room_nights(i,j) == res_ID){
						rmanager->reset_room_nights(i,j);
					}
				}
			}
		}
		else if (option == 3){
			cout << "List of bookings: " << endl;
			cout << "Room:    ";
			for (int j = 1; j <= 20; j ++) {

				cout << setw(3) << j;
				cout << " | ";
			}
			cout << endl;
			for (int i = 0; i < 7; i++) {
				cout << "March ";
				int k = i + 1;
				cout << k;
				cout << ": ";
				for (int j = 0; j < 20; j++) {
					cout << setw(3) << rmanager->get_room_nights(i, j);
					cout <<  " | ";
				}
				cout << endl;
			}

		}
		else
			cout << "Goodbye !" << endl;

	}
	return 0;
}
