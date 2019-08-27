slidenumbers: true
autoscale: true
footer: @AdamMc331<br/>#DCNYC19
build-lists: true

# //TODO: Write A Better Comment
## Adam McNeilly - @AdamMc331

---

![inline](java_tweet.png)

---

# This Is Bad Advice

---

> "When you need to write a comment, it usually means that you have failed to write code that was expressive enough. You should feel a sharp pain in your stomach every time you write a comment."
--

^ Unemphathetic, not a culture we want to cultivate, discouraging 

---

# You Are Not A Failure For Writing Comments

---

# We Need To Stop Writing **Bad** Comments

---

# Why Do We Have Comments?

---

# To Provide Additional Insight

```kotlin
/**
 * There is certain functionality that we need to be consistent 
 * in all WebViews of our app.
 * For some URLs, though, we need additional customization so we 
 * can extend this base class accordingly.
 */
class BaseWebViewClient(...) : WebViewClient
```

---

# To Explain Why We Did Something

```kotlin
// The API returns the time in seconds
// but we need to manipulate it as milliseconds.
val timeInMillis = response.time * 1000
```

---

# To Provide Documentation

```kotlin
interface AccountDAO {
    /**
     * Inserts an account into the database.
     *
     * @param[account] The account that we're inserting.
     * @return The ID of the inserted account.
     */
    fun insert(account: Account): Long
}
```

---

# What Risks Do Comments Pose?

---

# Changing Code Doesn't Guarantee We Change Comments

---

# Describe Some Action

```kotlin
// We only want active users
val usersToDisplay = userList.filter { user ->
	user.isActive
}
```

---

![inline](threeweekslater.png)

---

# That Action Changed

```kotlin
// We only want active users
val usersToDisplay = userList.filter { user ->
	user.isActive && user.completedRegistration
}
```

---

![inline](muchlater.jpg)

---

# Which Is Right? 🤔

```kotlin
// We only want active users
val usersToDisplay = userList.filter { user ->
	user.isActive && user.completedRegistration
}
```

---

# Three Types Of Comments

- Comments that are unnecessary
- Comments that are unhelpful
- Comments that are helpful

---

# Unnecessary Comments

---

Bad: Repeating the code

```kotlin
interface AccountDAO {
    /**
     * Inserts an account into the database.
     *
     * @param[account] The account that we're inserting.
     * @return The ID of the inserted account.
     */
    fun insert(account: Account): Long
}
```

---

Good: Remove what we don't need

```kotlin
interface AccountDAO {
    /**
     * @return The ID of the inserted account.
     */
    fun insert(account: Account): Long
}
```

^ Exception: Public API

---

# Change Code To Avoid Needing Comments

---

Good: Clarifying Behavior

```kotlin
// Saves data to database
fun saveData() {
	// ...
}
```

Better: More Expressive Code

```kotlin
fun saveDataToDatabase() {
	// ...
}
```

---

Good: Break Up Method

```kotlin
fun transferMoney(fromAccount: Account, toAccount: Account, amount: Double) {
   // create withdrawal transaction and remove from fromAccount
   // ...

   // create deposit transaction and add from toAccount
   // ...
}
```

Better: Extract Functionality

```kotlin
fun transferMoney(fromAccount: Account, toAccount: Account, amount: Double) {
   withdrawMoney(fromAccount, amount)
   depositMoney(toAccount, amount)
}
```

---

# Now What?

- We removed any redundant comments
- We changed code to avoid comments
- How do we ensure the comments we do write are helpful?

---

# Comments Tell You **Why**, Code Tells You **What**

---

# Comments That Tell Us What

```kotlin
/**
 * A list of updated questions to be replaced in our list by an interceptor.
 */
private val updatedQuestions: MutableMap<Long, Question> = HashMap()
```

---

# Comments That Tell Us Why

```kotlin
/**
 * The `PagedList` class from Android is backed by an immutable list. 
 * However, if the user answers a question locally, we want to update the display 
 * without having to fetch the data from the network again.
 * 
 * To do that, we keep this local cache of questions that the user has 
 * answered during this app session, and later when we are building 
 * the list we can override questions with one from this list, if it exists.
 * That's determined by the key of this HashMap, which is the question ID.
 */
private val updatedQuestions: MutableMap<Long, Question> = HashMap()
```

---

# Comments With Examples

---

Okay: No Examples

```kotlin
class Pokedex {
    /**
     * @param[name] The name of the Pokemon.
     */
    fun addPokemon(name: String, number: Int) {

    }
}
```

Better: With Examples

```kotlin
class Pokedex {
    /**
     * @param[name] The name of the Pokemon (Bulbasaur, Ivysaur, Venusaur).
     */
    fun addPokemon(name: String, number: Int) {

    }
}
```

---

# Links To Additional Resources

---

# To StackOverflow

```kotlin
/**
 * A ViewPager that cannot be swiped by the user, 
 * but only controlled programatically.
 *
 * Inspiration: https://stackoverflow.com/a/9650884/3131147
 */
class NonSwipeableViewPager(
	context: Context, 
	attrs: AttributeSet? = null
) : ViewPager(context, attrs) {
	// ...
}
```

---

# To Internal Documentation

```kotlin
/**
 * Implementation of some feature that I was asked to build.
 *
 * Design/Product Spec: https://confluence.com/some/feature
 */
class SomeFeatureFragment : Fragment() {
	// ...
}
```

---

# To Reported Issues

```kotlin
/**
 * The carousel library we use does not support a
 * specific functionality that we need. We've extended
 * this class to modify it ourselves.
 *
 * Issue reported: https://github.com/library/issues/1
 */ 
class MyCustomCarousel : Carousel() {
	// ...
}
```

^ Work Arounds Or Otherwise

---

# Actionable Comments

---

# //TODO: Comments

If you're not going to do it now, create accountability with links to issue trackers.

```kotlin
//TODO: Consolidate both of these classes
// since we only have one activity now.
// AAA-123
class MainActivity : BaseActivity() {
	// ...
}
```

---

# Deprecation Comments

---

Bad: No Explanation

```kotlin
@Deprecated
public interface DefaultBehavior {
    // ...
}
```

Better: Provide Alternative

```kotlin
/**
 * @deprecated Use {@link AttachedBehavior} instead
 */
@Deprecated
public interface DefaultBehavior {
    // ...
}
```

---

# Other General Suggestions

---

# ASCII Art?[^2]

![inline](ascii_comment.png)

[^2]: https://github.com/material-components/material-components-android/blob/master/lib/java/com/google/android/material/chip/ChipDrawable.java#L130-L151

---

# Summarize Large Sections Of Code

![inline](code_summary.png)

---

# Reference Definitions

- Helps survive refactoring of a definition
- IDE lets you click into references
- Will create links for auto generated

---

# Without References

```kotlin
/**
 * Retrieves the primary Type for a Pokemon.
 */
val firstType: Type?
    get() = currentState.pokemon?.sortedTypes?.firstOrNull()
```

Refactor class name...

[.code-highlight: 2, 4]
```kotlin
/**
 * Retrieves the primary Type for a Pokemon.
 */
val firstType: PokemonType?
    get() = currentState.pokemon?.sortedTypes?.firstOrNull()
```

---

# With References

```kotlin
/**
 * Retrieves the primary [Type] for a [Pokemon].
 */
val firstType: Type?
    get() = currentState.pokemon?.sortedTypes?.firstOrNull()
```

Refactor class name...

[.code-highlight: 2, 4]
```kotlin
/**
 * Retrieves the primary [PokemonType] for a [Pokemon].
 */
val firstType: PokemonType?
    get() = currentState.pokemon?.sortedTypes?.firstOrNull()
```

---

# Use Consistent Language

- When documenting methods that return booleans, try to always describe the true condition
- Don't describe the true response for some methods and the false response for others

---

# Inconsistent Documentation

```kotlin
/**
 * @return True if the user has signed on within the last 24 hours.
 */
fun isActive(): Boolean {
    // ...
}

/**
 * @return False if the user is not a staff member for our team.
 */ 
fun isStaff(): Boolean {
    // ...
}
```

---

# Consistent Documentation

```kotlin
/**
 * @return True if the user has signed on within the last 24 hours.
 */
fun isActive(): Boolean {
    // ...
}

/**
 * @return True if the user is a staff member of our team.
 */ 
fun isStaff(): Boolean {
    // ...
}
```

---

# Recap

- Remove redundant comments
- Write expressive code
- Write helpful comments
 - Explain **why**
 - Provide examples
 - Give guidance
 - Leverage IDE tools
 - Use consistent language

---

# Thank You!

## https://github.com/AdamMc331/TODO-DCNYC19