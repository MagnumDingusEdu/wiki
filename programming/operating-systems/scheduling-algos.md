---
title: Scheduling Algos
description: 
published: true
date: 2021-07-11T20:47:02.739Z
tags: 
editor: markdown
dateCreated: 2021-07-11T20:47:02.739Z
---

# Scheduling Algos
```cpp
#include <iostream>
#include <vector>
#include <sstream>
#include <queue>

enum CHOICES {
    FIRST_COME_FIRST_SERVE = 1,
    SHORTEST_JOB_FIRST_P,
    SHORTEST_JOB_FIRST_NP,
    PRIORITY_P,
    PRIORITY_NP,
    ROUND_ROBIN
};

enum P_CHOICES {
    NON_PREEMPTIVE = 0,
    PREEMPTIVE
};

struct Process {
    int pid;
    int arr_time;
    int burst_time;
    int priority;

    int turn_arr_time;
    int comp_time;
    int waiting_time;

    int remaining_burst_time;
    int time_executed;


    Process(int process_id, int arrival, int burst) {
        this->pid = process_id;
        this->arr_time = arrival;
        this->burst_time = burst;
        this->turn_arr_time = -1;
        this->comp_time = -1;
        this->waiting_time = -1;
        this->remaining_burst_time = this->burst_time;
        this->time_executed = -1;
        this->priority = -1;
    }
};

void displayProcesses(const std::vector<Process> &process_list, bool display_priority = false) {
    std::cout << "Process id\tArrival Time\tBurst Time\tWaiting Time\tTurn Around Time\tCompletion Time";
    std::cout << (display_priority ? "\tPriority" : "") << std::endl;
    for (auto x : process_list) {
        std::cout << x.pid << "\t\t\t" << x.arr_time << "\t\t\t\t" << x.burst_time << "\t\t\t" << x.waiting_time
                  << "\t\t\t\t" << x.turn_arr_time << "\t\t\t\t\t" << x.comp_time;
        if (display_priority)
            std::cout << "\t\t\t\t\t" << x.priority;
        std::cout << std::endl;
    }


}

void mergeSortProcesses(std::vector<Process> &left, std::vector<Process> &right, std::vector<Process> &process_list) {
    int left_size = left.size();
    int right_size = right.size();

    int main_index = 0;
    int left_index = 0;
    int right_index = 0;

    while (left_index < left_size && right_index < right_size) {
        if (left[left_index].arr_time == right[right_index].arr_time) {
            if (left[left_index].pid < right[right_index].pid)
                process_list[main_index] = left[left_index++];
            else
                process_list[main_index] = right[right_index++];
        } else if (left[left_index].arr_time < right[right_index].arr_time) {
            process_list[main_index] = left[left_index++];

        } else {
            process_list[main_index] = right[right_index++];
        }
        main_index++;
    }

    while (left_index < left_size)
        process_list[main_index++] = left[left_index++];
    while (right_index < right_size)
        process_list[main_index++] = right[right_index++];
}

void sortProcesses(std::vector<Process> &process_list) {
    if (process_list.size() <= 1)
        return;

    unsigned short int mid = process_list.size() / 2;
    std::vector<Process> left;
    std::vector<Process> right;

    left.reserve(mid);
    for (int i = 0; i < mid; ++i)
        left.push_back(process_list[i]);

    right.reserve(process_list.size() - mid);
    for (int i = mid; i < process_list.size(); ++i)
        right.push_back(process_list[i]);

    sortProcesses(left);
    sortProcesses(right);
    mergeSortProcesses(left, right, process_list);
}

std::stringstream FirstComeFirstServe(std::vector<Process> &_processQueue, int scheduling_overhead = 0) {
    sortProcesses(_processQueue);


    int currentTime = scheduling_overhead + _processQueue[0].arr_time; // set to the arrival time of first process
    int totalIdleTime = currentTime - 0; // add idle time in case first process didn't arrive at zero
    std::stringstream process_order;

    for (auto &process: _processQueue) {
        if (currentTime <= process.arr_time) {


            totalIdleTime += process.arr_time - currentTime;
            currentTime = process.arr_time + process.burst_time;
            process.waiting_time = 0;
            process.comp_time = currentTime;
            process.turn_arr_time = process.comp_time - process.arr_time;
        } else {
            process.waiting_time = currentTime - process.arr_time;
            currentTime += process.burst_time;
            process.comp_time = currentTime;
            process.turn_arr_time = currentTime - process.arr_time;

        }
        process_order << "P" << process.pid << " ➙ ";
        currentTime += scheduling_overhead;
        totalIdleTime += scheduling_overhead;
    }
    return process_order;
}

std::stringstream ShortestJobFirst(std::vector<Process> &_processQueue, bool preemptive) {
    sortProcesses(_processQueue);


    int currentTime = 0;
    int process_counter = _processQueue.size();
    int smallestProcessID = 0;
    int minBurstTime;
    int prevProcessID = -1;
    std::stringstream process_order;
    bool reevaluate_flag = false;
    while (process_counter > 0) {
        minBurstTime = 99999;
        for (int i = 0; i < _processQueue.size(); ++i) {
            if (!preemptive) {
                if (_processQueue[i].remaining_burst_time == 0) {
                    _processQueue[i].remaining_burst_time = -1;
                    _processQueue[i].comp_time = currentTime;
                    process_counter--;
                    reevaluate_flag = true;
                }
            } else if (_processQueue[i].arr_time <= currentTime && _processQueue[i].remaining_burst_time > 0) {
                if (_processQueue[i].remaining_burst_time < minBurstTime) {
                    minBurstTime = _processQueue[i].remaining_burst_time;

                    smallestProcessID = i;
                }
            } else if (_processQueue[i].remaining_burst_time == 0) {
                _processQueue[i].remaining_burst_time = -1;
                _processQueue[i].comp_time = currentTime;
                process_counter--;
            }
        }

        if (reevaluate_flag) {
            for (int i = 0; i < _processQueue.size(); ++i) {
                if (_processQueue[i].arr_time <= currentTime && _processQueue[i].remaining_burst_time > 0) {
                    if (_processQueue[i].remaining_burst_time < minBurstTime) {
                        minBurstTime = _processQueue[i].remaining_burst_time;
                        smallestProcessID = i;
                    }
                }
            }
            reevaluate_flag = false;
        }

        if (prevProcessID != smallestProcessID) {
            _processQueue[smallestProcessID].waiting_time = currentTime;
            _processQueue[smallestProcessID].time_executed =
                    _processQueue[smallestProcessID].burst_time - _processQueue[smallestProcessID].remaining_burst_time;
            process_order << "P" << _processQueue[smallestProcessID].pid << " ➙ ";
        }
        prevProcessID = smallestProcessID;
        ++currentTime;
        _processQueue[smallestProcessID].remaining_burst_time--;

    }
    return process_order;
}

std::stringstream PriorityScheduling(std::vector<Process> &_processQueue, bool zeroHighest, bool preemptive) {
    std::cout << "Please enter the process priorities: ";
    for (auto &x : _processQueue)
        std::cin >> x.priority;

    sortProcesses(_processQueue);

    std::stringstream process_order;
    int currentTime = 0;
    int process_counter = _processQueue.size();
    int currentProcessID = 0;
    int prevProcessID = -1;
    int minPriority;
    int maxPriority;

    bool checkProcesses = false;

    while (process_counter > 0) {
        minPriority = 99999;
        maxPriority = -1;
        for (int i = 0; i < _processQueue.size(); ++i) {
            if (!preemptive) {
                if (_processQueue[i].remaining_burst_time == 0) {
                    _processQueue[i].remaining_burst_time = -1;
                    _processQueue[i].comp_time = currentTime;
                    process_counter--;
                    checkProcesses = true;
                }
            } else if (_processQueue[i].arr_time <= currentTime && _processQueue[i].remaining_burst_time > 0) {
                if (zeroHighest) {
                    if (_processQueue[i].priority < minPriority) {
                        minPriority = _processQueue[i].priority;
                        currentProcessID = i;
                    }
                } else {
                    if (_processQueue[i].priority > maxPriority) {
                        maxPriority = _processQueue[i].priority;
                        currentProcessID = i;
                    }
                }
            } else if (_processQueue[i].remaining_burst_time == 0) {
                _processQueue[i].remaining_burst_time = -1;
                _processQueue[i].comp_time = currentTime;
                process_counter--;
            }
        }

        if (checkProcesses) {
            for (int i = 0; i < _processQueue.size(); ++i) {
                if (_processQueue[i].arr_time <= currentTime && _processQueue[i].remaining_burst_time > 0) {
                    if (zeroHighest) {
                        if (_processQueue[i].priority < minPriority) {
                            minPriority = _processQueue[i].priority;
                            currentProcessID = i;
                        }
                    } else {
                        if (_processQueue[i].priority > maxPriority) {
                            maxPriority = _processQueue[i].priority;
                            currentProcessID = i;
                        }
                    }
                }
            }
            checkProcesses = false;
        }

        if (prevProcessID != currentProcessID) {
            process_order << "P" << _processQueue[currentProcessID].pid << " ➙ ";
            _processQueue[currentProcessID].waiting_time = currentTime;
            _processQueue[currentProcessID].time_executed =
                    _processQueue[currentProcessID].burst_time - _processQueue[currentProcessID].remaining_burst_time;
        }
        prevProcessID = currentProcessID;
        ++currentTime;
        _processQueue[currentProcessID].remaining_burst_time--;

    }

    return process_order;
}

std::stringstream RoundRobin(std::vector<Process> &_processQueue, int time_quantum) {

    sortProcesses(_processQueue);


    int total_burst_time_required = 0;
    for (auto &x : _processQueue)
        total_burst_time_required += x.burst_time;

    int currentTime = 0;
    int currentProcess = -1;
    int previousProcess = -1;
    int total_processes = _processQueue.size();
    std::stringstream process_order;

    std::queue<int> _readyQueue;
    int maxArrivalTime = _processQueue.back().arr_time;


    int timer_ticker = time_quantum;

    while (total_processes > 0) {

        for (size_t i = 0; i < _processQueue.size(); ++i)
            if (currentTime == _processQueue[i].arr_time)
                _readyQueue.push(i);


        if (currentTime < maxArrivalTime) {
            if (_readyQueue.empty()) {

                currentTime++;
                continue;
            } else if (currentProcess < 0) {
                currentProcess = _readyQueue.front();
                _readyQueue.pop();
            }
        } else if (maxArrivalTime == 0) {

            if (currentProcess < 0) {
                currentProcess = _readyQueue.front();
                _readyQueue.pop();
            }

        }

        if (_processQueue[currentProcess].remaining_burst_time == 0) {

            total_processes--;
            _processQueue[currentProcess].comp_time = currentTime;
            _processQueue[currentProcess].turn_arr_time = currentTime - _processQueue[currentProcess].arr_time;
            _processQueue[currentProcess].waiting_time =
                    _processQueue[currentProcess].turn_arr_time - _processQueue[currentProcess].burst_time;


            if (_readyQueue.empty()) {
                break;
            }
            currentProcess = _readyQueue.front();
            timer_ticker = time_quantum;
            _readyQueue.pop();


        } else if (timer_ticker == 0) {
            _readyQueue.push(currentProcess);

            currentProcess = _readyQueue.front();
            _readyQueue.pop();
            timer_ticker = time_quantum;
            _processQueue[currentProcess].waiting_time = currentTime;

        }
        if (previousProcess != currentProcess) {
            process_order << "P" << _processQueue[currentProcess].pid << " ➙ ";

        }
        previousProcess = currentProcess;
        _processQueue[currentProcess].remaining_burst_time--;
        timer_ticker--;
        currentTime++;


    }

    return process_order;
}

int main() {
    std::vector<Process> _processQueue;
    std::cout << "Welcome to Vividh's Process Scheduler, written in C++17." << std::endl;

    std::cout << "Please enter the number of processes : ";

    int size;
    std::cin >> size;

    int *process_ids = new int[size];
    int *arrival_times = new int[size];
    int *burst_times = new int[size];

    std::cout << "Please enter the process IDs (integers only): ";
    for (int i = 0; i < size; ++i)
        std::cin >> process_ids[i];

    std::cout << "Please enter the arrival times of the processes: ";
    for (int i = 0; i < size; ++i)
        std::cin >> arrival_times[i];

    std::cout << "Please enter the burst times of the processes: ";
    for (int i = 0; i < size; ++i)
        std::cin >> burst_times[i];


    _processQueue.reserve(size);
    for (int i = 0; i < size; ++i) {
        _processQueue.emplace_back(process_ids[i], arrival_times[i], burst_times[i]);
    }
    delete[] process_ids;
    delete[] burst_times;
    delete[] arrival_times;
    std::stringstream process_order, welcome_message;

    short int choice;


    welcome_message << "Please select an algorithm: \n\n"
                       "1) First Come First Serve (FCFS)\n"
                       "2) Shortest Job First (Preemptive)\n"
                       "3) Shortest Job First (Non-Preemptive)\n"
                       "4) Priority based Scheduler (Preemptive)\n"
                       "5) Priority based Scheduler (Non-Preemptive)\n"
                       "6) Round Robin Algorithm (Preemptive)\n" << std::endl;

    welcome_message << "Your choice : ";

    std::cout << welcome_message.str();
    std::cin >> choice;

    switch (choice) {

        case FIRST_COME_FIRST_SERVE: {
            std::cout << "Please enter context switch overhead for FCFS, if any (enter 0 for none): ";
            int overhead;
            std::cin >> overhead;
            if (overhead < 0)
                overhead = 0;
            process_order = FirstComeFirstServe(_processQueue, overhead);
            break;
        }
        case SHORTEST_JOB_FIRST_P:
            process_order = ShortestJobFirst(_processQueue, PREEMPTIVE);
            break;
        case SHORTEST_JOB_FIRST_NP:
            process_order = ShortestJobFirst(_processQueue, NON_PREEMPTIVE);
            break;
        case PRIORITY_P: {
            std::cout << "Please choose the priority order : \n"
                         "0) Zero is lowest priority\n"
                         "1) Zero is highest priority"
                      << std::endl;
            int zero_choice;
            std::cout << "Your choice : ";
            std::cin >> zero_choice;
            process_order = PriorityScheduling(_processQueue, zero_choice, PREEMPTIVE);
            break;
        }
        case PRIORITY_NP: {
            std::cout << "Please choose the priority order : \n"
                         "0) Zero is lowest priority\n"
                         "1) Zero is highest priority"
                      << std::endl;
            int zero_choice;
            std::cout << "Your choice : ";
            std::cin >> zero_choice;
            process_order = PriorityScheduling(_processQueue, zero_choice, NON_PREEMPTIVE);
            break;
        }
        case ROUND_ROBIN: {
            std::cout << "Please enter the time quantum : ";
            int time_quantum;
            std::cin >> time_quantum;
            process_order = RoundRobin(_processQueue, time_quantum);
            break;
        }
        default:
            std::cout << "Invalid choice of algorithm. Exiting.";
            exit(-1);

    }


    int totalWaitTime = 0;
    int totalTurnAroundTime = 0;
    int totalCompletionTime = 0;
    for (auto &x : _processQueue) {
        x.turn_arr_time = x.comp_time - x.arr_time;
        if (choice == FIRST_COME_FIRST_SERVE ||
            choice == PRIORITY_NP ||
            choice == SHORTEST_JOB_FIRST_NP ||
            choice == ROUND_ROBIN) {
            x.waiting_time = x.turn_arr_time - x.burst_time;

        } else if (choice == PRIORITY_P || choice == SHORTEST_JOB_FIRST_P) {
            x.waiting_time = x.waiting_time - x.time_executed - x.arr_time;
        }

        totalWaitTime += x.waiting_time;
        totalTurnAroundTime += x.turn_arr_time;
        totalCompletionTime += x.comp_time;
    }


    displayProcesses(_processQueue, (choice == PRIORITY_NP || choice == PRIORITY_P));


    std::cout << "Order of processes : " << process_order.str().substr(0, process_order.str().size() - 5) << "\n";
    std::cout << "Average waiting time : " << (float) totalWaitTime / (float) _processQueue.size() << " ms\n";
    std::cout << "Average turn around time : " << (float) totalTurnAroundTime / (float) _processQueue.size() << " ms\n";
    std::cout << "Average completion time : " << (float) totalCompletionTime / (float) _processQueue.size() << " ms"
              << std::endl;


}
```