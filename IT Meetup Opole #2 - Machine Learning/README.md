# IT Meetup Opole #2 - Machine Learning
### [Link to the event info ](https://itmeetupopole2.evenea.pl/)

On March 21, 2019, the next meeting of the IT ITETET - Opole series will take place in the Science and Technology Park in Opole (2 Technologiczna Street).
The hosts of the meeting will be Park Naukowo-Technologiczny w Opolu Sp. z o.o. and Opole NUTRICIA belonging to the DANONE group.
In connection with the Opole programming marathon "Hackathon DANONE AI Masters Opole 2019" planned for 12-13 April, we invited a lecturer who will bring closer the subject of artificial intelligence and machine learning.

Speaker of "IT MeetUp Opole - Machine Learning" will be: _Jose Gregorio Ferreira_

div style="text-align:center"><img src ="notebooks/img/Danone.png" /></div>
_______________________

# Environment set-up: Python
Thanks to the wonderful job the guys from Anaconda put together, getting ready to start solving data challenges is very straight forward.
I am assuming you already have anaconda installed in your system. If not, go to [Anaconda distributions](https://www.anaconda.com/distribution/), and get the one that best suits you.

Either you had it installed or just finished the installation, please follow these steps in the command prompt:
1.- conda update conda
2.- conda update -n base -c defaults conda
3.- conda create -n yourenvname python=3.7 anaconda
4.- conda install nb_conda_kernels
5.- Running jupyer:
	5.1- In a stand alone system, just execute jupyer from anaconda navigator or just type in the command prompt: jupyter notebook.
	5.2- If you are using a remote Linux machine please run: jupyter notebook --no-browser --port=8022, and then in a separated console run ssh -L 8022:127.0.0.1:8022 hack@5.226.96.156 -p 8022 to open an SSH tunnel.
6.- Finally, copy the link that jupyter generated for you, together with a token, and paste in the browser of your local system
