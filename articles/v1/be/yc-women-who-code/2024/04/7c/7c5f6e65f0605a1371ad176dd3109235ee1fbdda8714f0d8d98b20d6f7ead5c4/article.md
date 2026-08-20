---
schema_version: "1.0.0"
document_id: "7c5f6e65f0605a1371ad176dd3109235ee1fbdda8714f0d8d98b20d6f7ead5c4"
company_key: "yc-women-who-code"
company: "Women Who Code"
source_id: "yc-women-who-code-news-import-daedbdc33962"
canonical_url: "https://womenwhocode.com/blog/how-to-submit-a-form-in-react-using-formdata/"
published_at: "2024-04-17T06:00:00+00:00"
first_seen_at: "2026-07-26T05:38:05.914257+00:00"
fetched_at: "2026-07-28T22:25:56.819128+00:00"
content_hash: "sha256:b12e5952a176a3951f39f71cbf132252a641a7b86c0d9a84e61a4568b68f5ea9"
---

# How to Submit a Form in React Using FormData

Form is one of the most essential components in any web application, expanding from registration to submitting feedback or inquiry. Form comes with one important topic i.e., the number of elements, which can vary from username and password in the login form to the collection of multiple data in submitting an application. All these entries need to be validated to have a successful submission or show the exact error in the relevant field in case of error. With a growing number of validations comes the hurdle of maintenance. And here comes another browser supporting constructor function called **FormData** . This is neither a custom feature nor created by the React community.


**Let’s Implement**


Let’s create a simple form to add an expense, where it takes only three inputs: the transaction date, the amount a person has spent and the category on which the person has made the expense. These input fields must have a **name** attribute in it:


<form>


<input type=”date” name=”transactionDate” />


<input type=”decimal” name=”amount” />


<select name=”category”>


<option value=””>Select Category</option>


<option>Food</option>


<option>Transport</option>


<option>Entertainment</option>


<option>Other</option>


</select>


<button>Submit Expense</button>


</form>


**Time for Some Validation**


The default validations we can add as supported by HTML are \`required\`,\`min\`, \`max\`. The validations need not have to be hardcoded values as in the example but can be customized as per requirement. Now, the form updates to:


“\`


<form onSubmit={addExpenseHandler}>


<input type=”date” name=”expenseDate” max={“2024-12-31”} min={“2024-01-01”} required/>


<input type=”decimal” placeholder=”Please Enter Amount” name=”expenseAmount” required max={1000} min={2}/>


<select name=”expenseCategory” required>


<option value=””>Select Category</option>


<option value=”Food”>Food</option>


<option value=”Transport”>Transport</option>


<option value=”Entertainment”>Entertainment</option>


<option value=”Other”>Other</option>


</select>


<button>Add Expense</button>


</form>


“\`


**Now Let’s Submit**


const addExpenseHandler = (event) => {


event.preventDefault();


const expense = new FormData(event.target);


const expenseDetails = Object.fromEntries(expense);


const expenseCategory = expense.getAll(“expenseCategory”);


expenseDetails.expenseCategory = expenseCategory;


expenseDetails.id = expenses.length + 1;


setExpenses((prev) => \[…prev, expenseDetails\]);


event.target.reset();


};


That’s a lot of code — let’s go line by line.


Now comes the final action, which is submitting the expense that we have entered, and for that, we will use \`addExpenseHandler\` function. As a submission event bubbles up in HTML, we need to prevent it using:


\`event.preventDefault();\`


In this submit action, the \`event.target\` is now the entire \`form\`. Here comes the constructor function \`FormData,\` which takes this form as input and returns the key-value pair of the form entries:


\`const expense = new FormData(event.target);\`


To retrieve these key-value pair we need to call:


\`Object.fromEntries(expense)\` .


Now, for inputs like \`select\` where multiple options are present for a single **name** attribute, we need to run a \`getAll\` function on the relevant **name** attribute like this:


\`const expenseCategory = form.getAll(“category”);\`


Now this property aka \`expenseCategory\` can be added to the expenseDetails object:


\`expenseDetails.category = category;\`


Now we can update the list of expenses we have created using:


\`setExpenses((prev) => \[…prev, expenseDetails\]);\`


And finally, we can reset the form to the default state using:


\`event.target.reset();\`


**Advantages**


1. We don’t have to set a state for the individual form elements nor a binded state for all the form elements like this:


\`


const \[category, setCategory\] = useState(“”);


or


const \[expense, setExpense\] = useState({ amount: 0.0, transactionDate: “”, category: “”});


\`


2. We don’t need a function to handle the changes made to these form elements.


3. We can leverage the existing validations supported by HTMLForm elements.


4. This way, our code is more concise and scalable to add more form elements.


**Disadvantages**


1. This approach is not right if we need validation on every keystroke. Here, we will learn the errors only when the form is finally submitted.


2. The inbuilt validations of \`max\` or \`min\` still fail here if we are not restricting them explicitly. The user can still type a value more than max or less than the min set.


Along with the other alternative patterns to submit forms, FormData saves a lot of time. There is less boilerplate code to write in forms that are constantly increasing in form element length.


Though we have discussed the disadvantages of it and scenarios where it won’t fit, we can easily use it in the other scenarios.


[Find the entire code base here.](https://github.com/poulami-saha/expense-tracker-blog/blob/main/src/App.js)
