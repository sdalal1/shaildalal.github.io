---
layout: page
title: AI-Bot
description: AI bot for Covid-19
img: assets/img/Neural_net_image.png
importance: 6
category: work
---

## Simplifying Campus Safety: Neural-19, the COVID-19 Chatbot for University Dorms

#### Introduction:
Neural-19 emerged as a chatbot tailored to tackle COVID-19 challenges within university dormitories. By amalgamating web scraping, speech recognition, and mask detection, Neural-19 aimed to furnish vital information and fortify safety measures.

#### Problem Statement:
With COVID-19 disrupting campus life, ensuring dormitory safety became paramount. Neural-19 sought to provide clarity on policies, dispense information, and uphold safety protocols.

#### Functionality Overview:
Neural-19's functionalities comprised:

1. **Real-Time Data Retrieval:** It sourced live COVID-19 statistics, policy updates, testing sites, and vaccination centers.
2. **Mask Detection:** Leveraging OpenCV and TensorFlow, Neural-19 identified mask compliance in real-time, bolstering safety measures.
3. **Speech Recognition:** Users interacted vocally through Google's Speech Recognition API, enhancing accessibility.
4. **User Query Handling:** Neural-19 processed and responded to user queries efficiently, fostering understanding and engagement.

#### Technological Framework:
Neural-19 utilized:

- **TensorFlow + Keras:** These frameworks facilitated the creation and training of the chatbot's neural network, enabling it to comprehend and respond to user queries effectively.
- **OpenCV:** Employed for mask detection, OpenCV provided a robust platform for real-time analysis of video streams, detecting whether individuals were wearing masks.
- **scikit-learn:** This library aided in developing classification models, particularly for mask detection, ensuring accurate identification.
- **Google API:** Integration of Google's Speech Recognition API allowed users to interact with the chatbot through spoken commands, enhancing user experience and accessibility.

#### Development Timeline:
The development phases of Neural-19 encompassed:

1. **Chatbot Training and GUI Development:** This stage involved creating a labeled dataset to train the chatbot, alongside developing a user-friendly graphical interface using Tkinter.
2. **Speech Recognition Integration:** The integration of Google's Speech Recognition API enabled the chatbot to process voice commands, expanding its usability and accessibility.
3. **Mask Detection Training and Integration:** Neural-19 underwent training for mask detection models, incorporating OpenCV for real-time analysis, and seamlessly integrating it with the chatbot for cohesive functionality.
4. **Socket Implementation:** To facilitate communication between independently running programs, such as the chatbot and mask detection module, sockets were implemented to enable efficient data transfer.

#### Conclusion:
Neural-19 represented a robust solution to COVID-19 challenges within university dormitories. By leveraging advanced technologies and meticulous development, Neural-19 fostered a safer environment, exemplifying the potential of technology in addressing contemporary societal issues.
The implementation won second prize 


<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include video.html path="assets/video/Team_15_AI_Chatbot_Demo.mp4" class="img-fluid rounded z-depth-1" controls=true autoplay=true %}
    </div>
</div>