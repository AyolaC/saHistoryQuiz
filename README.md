# saHistoryQuiz
History true or false quiz app


Welcome photo:

![Welcome page](https://github.com/user-attachments/assets/fdca7fb1-fe78-4920-b1f8-7bbede973649)

Flashcreen photo:

![flashscreen page](https://github.com/user-attachments/assets/ca1cc09f-7b81-4ccb-a01b-4f3104db2534)

Score screen photo:

![Score screen page](https://github.com/user-attachments/assets/76f0543a-0635-4318-9a2d-b50e5f2a8e0d)

Reveiw answers screen photo:

![review answers page](https://github.com/user-attachments/assets/8e27de67-9e64-477c-9d43-797e5d4a45af)

# saHistoryQuiz Report:
Purpose of the App:
The saHistoryQuiz was designed to help users learn and test their knowledge on history-South African,
using flashcards.The goal is to provide a simple and fun interactive way to revise the topic, with immediate feedback for each question.The app is aimed for all users interested in history.

# Key Features:

True/False fllashcard questions

Immediate feedback("Correct"/"Incorrect")

Score tracking

Review screen showing all questions with their correct answers 

Final score display with personalised feedback

# Design Considerations:

User interface

XML- based layouts were used for compatibility and ease of visual structures in android studio app.

Buttons(True,False and Next) were disabled to prevent the user from selecting more than one response.

All screens are clear, user friendly and concise.

# Navigation:

Each screen (Welcome page, Score page and Review) is in a separate activity, with navigations done via intent.

THE App automatically goes to the sore page after the last question has been answered.

# Data Handling:

Questions and answers are stored in parallel arrays.

Data is passed between activities using Intent.putExtra().

# Error Handling:

The app handles invalid input and array out-of-bound issues.

Utillisation of Github and Github Actions

Commits were used to track changes

A detailed report with screenshots of the app.

# Conclusion:
The app demonstrates a user friendly design, proper code language, and modern development. Github ensured code quality and testing.
package com.example.quizy

import android.content.Intent
import android.os.Bundle
import android.widget.Button
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.activity.enableEdgeToEdge
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.foundation.layout.padding
import androidx.compose.material3.Scaffold
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.compose.ui.Modifier
import androidx.compose.ui.tooling.preview.Preview
import com.example.quizy.ui.theme.SaHistoryQuizTheme

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activitymain)

        //this button will take us to the next activity
        val btnStart = findViewById<Button>(R.id.btnStart)
        btnStart.setOnClickListener {
            val intent = Intent(this,MainActivity2::class.java)
            startActivity(intent)
        }
    }
}

<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#FFFFF59D"
    android:orientation="vertical"
    android:gravity="center"
    android:padding="24dp">

    <!-- Title -->
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="saHistoryQuiz"
        android:textColor="#FFF57F17"
        android:textSize="36sp"
        android:textStyle="bold"
        android:layout_marginBottom="8dp"/>

    <!-- Subtitle -->
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Test your South African history knowledge!"
        android:textColor="#FF9E9D24"
        android:textSize="18sp"
        android:layout_marginBottom="10dp"
        android:layout_gravity="center"
        />

    <!--Welcome Message-->

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:layout_marginBottom="32dp"
        android:text="Welcome to South Africa quiz ! Test your knowledge of South Africa's rich past. Let's see how well you know your history!"
        android:textColor="#FF9800"
        android:textSize="14sp" />
<!-- Start Button-->
    <Button
        android:id="@+id/btnStart"
        android:layout_width="200dp"
        android:layout_height="50dp"
        android:text="Start Quiz"
        android:textColor="#FF000000"
        android:backgroundTint="#FFFFD600"
        android:textSize="18sp"
        android:textStyle="bold"/>

</LinearLayout>

//flashcard screen
package com.example.quizy

import android.content.Intent
import android.os.Bundle
import android.os.Handler
import android.os.Looper
import android.widget.Button
import android.widget.TextView
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.activity.enableEdgeToEdge
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.foundation.layout.padding
import androidx.compose.material3.Scaffold
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.compose.ui.Modifier
import androidx.compose.ui.tooling.preview.Preview
import com.example.quizy.databinding.Activitymain2Binding
import com.example.quizy.ui.theme.SaHistoryQuizTheme
//this activity is the flashcard activity
class MainActivity2 : ComponentActivity() {

    private lateinit var binding: Activitymain2Binding
    private lateinit var questionTextView: TextView
    private lateinit var trueButton: Button
    private lateinit var falseButton: Button
    private lateinit var feedbackTextView: TextView
    private lateinit var nextButton: Button

    private val userAnswers = mutableListOf<Boolean>()

//theses are the statements that will show
    private val questionss = arrayOf(
        "Nelson Mandela was imprisoned on Robben Island for 27 years.",
        "The Anglo-Boer War ended in 1902 with a British victory.",
        "Jan van Riebeeck arrived at the Cape in 1652 to establish a Dutch colony.",
        "The Sharpeville Massacre occurred in 1960 during a protest against pass laws.",
        "South Africa became a democratic country in 1990."
    )

    //the users will be given these two options to answer the statements
    private val options = arrayOf(
        arrayOf("True", "False"),
        arrayOf("True", "False"),
        arrayOf("True", "False"),
        arrayOf("True", "False"),
        arrayOf("True", "False"),
        arrayOf("True", "False")
    )
//these are the correct answers fo the respective statements
    private val answers = booleanArrayOf(true, true, false, true, false)

    private var currentQuestionIndex = 0
    private var score = 0

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        binding = Activitymain2Binding.inflate(layoutInflater)
        setContentView(binding.root)

        questionTextView = findViewById(R.id.questionText)
        trueButton = findViewById(R.id.trueButton)
        falseButton = findViewById(R.id.falseButton)
        feedbackTextView = findViewById(R.id.feedbackText)
        nextButton = findViewById(R.id.nextButton)

        updateQuestion()


        trueButton.setOnClickListener {
            checkAnswer(true)
        }
        falseButton.setOnClickListener {
            checkAnswer(false)
        }
//next button will take user to the next question and sense when last question has been answered
        nextButton.setOnClickListener {
            if (currentQuestionIndex < questionss.size - 1)
                currentQuestionIndex++
            updateQuestion()
        }


    }


    private fun gotoMainActivity3() {
        val intent = Intent(this, MainActivity3::class.java)
        intent.putExtra("score", score)
        intent.putExtra("total", questionss.size)
        intent.putExtra("questions", questionss)
        intent.putExtra("answers", answers)
        intent.putExtra("userAnswer", userAnswers.toBooleanArray())
        startActivity(intent)
        finish()
    }


    private fun updateQuestion() {
        questionTextView.text = questionss[currentQuestionIndex]
        feedbackTextView.text = ""
        trueButton.isEnabled = true
        falseButton.isEnabled = true
    }

    private fun checkAnswer(userAnswer: Boolean) {
        val correctAnswer = answers[currentQuestionIndex]
        if (userAnswer == correctAnswer) {
            score++
            feedbackTextView.text = "correct"
        } else {
            feedbackTextView.text = "Incorrect"
        }
// this will restrict the user to one answer per statement
        trueButton.isEnabled = false
        falseButton.isEnabled = false

        if (currentQuestionIndex == questionss.size - 1) {
            Handler(Looper.getMainLooper()).postDelayed({ gotoMainActivity3() }, 1000)
        }
// this will restrict the user to one answer per statement
        trueButton.isEnabled = false
        falseButton.isEnabled = false

        userAnswers.add(userAnswer)






    }
}
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/quizLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="24dp"
    android:gravity="center"
    android:background="#FFFFF59D">

<!--- This is where statements will be displayed-->
    <TextView
        android:id="@+id/questionText"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Questions\nquestion"
        android:textSize="20sp"
        android:layout_marginBottom="24dp"/>

    <LinearLayout
        android:orientation="horizontal"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content">

        <!--True button-->

        <Button
            android:id="@+id/trueButton"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="True"
            android:backgroundTint="#49E70A"/>

        <!--False button-->

        <Button
            android:id="@+id/falseButton"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="False"
            android:layout_marginStart="16dp"
            android:backgroundTint="#E70A18"/>
    </LinearLayout>
    <!--Feedback on how the user has done will be displayed here-->
    <TextView
        android:id="@+id/feedbackText"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="feedback goes here"
        android:textSize="14sp"
        android:layout_marginTop="16dp"/>

    <!--Next button-->
    <Button
        android:id="@+id/nextButton"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Next"
        android:layout_marginTop="24dp"
        android:backgroundTint="#C4751212"/>
</LinearLayout>

//score screen
package com.example.quizy

import android.content.Intent
import android.os.Bundle
import android.widget.Button
import android.widget.TextView
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.activity.enableEdgeToEdge
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.foundation.layout.padding
import androidx.compose.material3.Scaffold
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.compose.ui.Modifier
import androidx.compose.ui.tooling.preview.Preview
import com.example.quizy.ui.theme.SaHistoryQuizTheme
class MainActivity3 : ComponentActivity() {
//this is the score screen
    private lateinit var scoreResultTextView: TextView
    private lateinit var feedbackTextView: TextView
    private lateinit var reviewButton: Button
    private lateinit var exitButton: Button

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

// this will enable the layout to be linked to this activity
            setContentView(R.layout.activitymain3)

            scoreResultTextView = findViewById(R.id.scoreResultTextView)
            feedbackTextView = findViewById(R.id.feedbackTextView)

            exitButton = findViewById(R.id.exitButton)
            reviewButton = findViewById(R.id.reviewButton)

            val score = intent.getIntExtra("score", 0)
            val total = intent.getIntExtra("total", 0)


            scoreResultTextView.text = "You $score out of $total correct"
//feedbacks that will display depending on the users amount of correct answers
            feedbackTextView.text = when {
                score == total -> "Perfect! Great job!"
                score >= total / 2 -> "Good effort!"
                else -> "Keep practicing and try again!"
            }

        val reviwButton = findViewById<Button>(R.id.reviewButton)
        reviwButton.setOnClickListener {
            val intent = Intent(this, MainActivity4::class.java)
            startActivity(intent)
        }
//allows users to exit the app
            exitButton.setOnClickListener {
                finishAffinity()
            }


        }
            }

        <?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/scoreLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="24dp"
    android:gravity="center"
    android:background="#FFFFF59D">
<!--Title-->
    <TextView
        android:id="@+id/scoreTitleTextView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Quiz Complete!"
        android:textSize="26sp"
        android:textStyle="bold"
        android:layout_marginBottom="24dp" />
    <!--the users score out of 5 will be display here-->
    <TextView
        android:id="@+id/scoreResultTextView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="You got X out of Y correct"
        android:textSize="20sp"
        android:layout_marginBottom="16dp" />
    <!--this is where feedback on how theyve done will be displayed-->
    <TextView
        android:id="@+id/feedbackTextView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Personalised feedback here."
        android:textSize="18sp"
        android:layout_marginBottom="32dp"
        android:textColor="#555555" />
    <!--Review Button-->
    <Button
        android:id="@+id/reviewButton"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Review Answers"
        android:layout_marginBottom="16dp" />
    <!--Exit Button-->
    <Button
        android:id="@+id/exitButton"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Exit App"
        android:backgroundTint="#D32F2F"
        android:textColor="#FFFFFF" />

</LinearLayout>    

//review screen
package com.example.quizy

import android.content.Intent
import android.os.Bundle
import android.os.Handler
import android.os.Looper
import android.util.Log
import android.widget.ArrayAdapter
import android.widget.Button
import android.widget.ListView
import android.widget.TextView
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.activity.enableEdgeToEdge
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.foundation.layout.padding
import androidx.compose.material3.Scaffold
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.compose.ui.Modifier
import androidx.compose.ui.tooling.preview.Preview
import com.example.quizy.databinding.Activitymain2Binding
import com.example.quizy.databinding.Activitymain4Binding
import com.example.quizy.ui.theme.SaHistoryQuizTheme

class MainActivity4 : ComponentActivity() {
//this screen is the review screen which will display the correct answers once the user is done
    private lateinit var exitButton2: Button

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activitymain4)

        //second exit button is included for users that chose to review answer sand want to exit app from there
        val exitButton2 = findViewById<Button>(R.id.exitButton2)


        exitButton2.setOnClickListener {
            finishAffinity()

        }

        }}

        <?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/reviewLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="16dp"
    android:orientation="vertical"
    android:background="#E0EB84">
 <!-- Title -->
   <TextView
       android:layout_width="wrap_content"
       android:layout_height="wrap_content"
       android:text="Review Answers"
       android:layout_gravity="center"
       android:textSize="32sp"
       android:textStyle="bold"
       android:textColor="#000000"/>
 <!--Question 1-->
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Nelson Mandela was imprisoned on Robben Island for 27 years."
        android:layout_gravity=""
        android:textSize="16sp"
        android:layout_marginTop="40dp"
        android:textColor="#000000"/>
 <!--Answer 1-->
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="True"
        android:textSize="20sp"
        android:textStyle="italic"
        android:textColor="#4AB825"/>
 <!--Question 2-->
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="The Anglo-Boer War ended in 1902 with a British victory."
        android:layout_gravity=""
        android:textSize="16sp"
        android:layout_marginTop="40dp"
        android:textColor="#000000"/>
 <!--Answer 2-->
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="True"
        android:textSize="20sp"
        android:textStyle="italic"
        android:textColor="#4AB825"/>
 <!--Question 3-->
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Jan van Riebeeck arrived at the Cape in 1652 to establish a Dutch colony."
        android:layout_gravity=""
        android:textSize="16sp"
        android:layout_marginTop="40dp"
        android:textColor="#000000"/>
 <!--Answer 3-->
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="False"
        android:textSize="20sp"
        android:textStyle="italic"
        android:textColor="#E93844" />
 <!--Question 4-->
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="The Sharpeville Massacre occurred in 1960 during a protest against pass laws."
        android:layout_gravity=""
        android:textSize="16sp"
        android:layout_marginTop="40dp"
        android:textColor="#000000"/>
 <!--Answer 4-->
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="True"
        android:textSize="20sp"
        android:textStyle="italic"
        android:textColor="#4AB825"/>
 <!--Question 5-->
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="South Africa became a democratic country in 1990."
        android:layout_gravity=""
        android:textSize="16sp"
        android:layout_marginTop="40dp"
        android:textColor="#000000"/>
 <!--Answer 5-->
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="False"
        android:textSize="20sp"
        android:textStyle="italic"
        android:textColor="#E93844"/>

 <!--Exit Button-->
    <Button
        android:id="@+id/exitButton2"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="86dp"
        android:background="#AF1D13"
        android:text="Exit"
        android:textColor="#80DEEA"
        />
</LinearLayout>

        







