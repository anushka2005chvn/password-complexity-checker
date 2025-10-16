# password-complexity-checker
import string
import re

def check_password_strength(password):
    """
    Assesses the strength of a password based on common security criteria.
    Returns a score and a detailed feedback report.
    """
    score = 0
    feedback = []

    # --- 1. Length Criteria (Max 5 points) ---
    length = len(password)
    if length >= 8:
        score += 2
        feedback.append("✅ Length (8+ characters): Passed (+2 points)")
    if length >= 12:
        score += 3  # Bonus points for extra length
        feedback[-1] = "✅ Length (12+ characters): Passed (+5 points total)"
    else:
        feedback.append(f"❌ Length ({length} characters): Needs at least 8 or preferably 12.")

    # --- 2. Character Set Criteria (Max 8 points) ---
    
    # Check for Lowercase Letters (Max 2 points)
    if any(c.islower() for c in password):
        score += 2
        feedback.append("✅ Contains lowercase letters (+2 points)")
    else:
        feedback.append("❌ Missing lowercase letters.")

    # Check for Uppercase Letters (Max 2 points)
    if any(c.isupper() for c in password):
        score += 2
        feedback.append("✅ Contains uppercase letters (+2 points)")
    else:
        feedback.append("❌ Missing uppercase letters.")

    # Check for Numbers/Digits (Max 2 points)
    if any(c.isdigit() for c in password):
        score += 2
        feedback.append("✅ Contains numbers (+2 points)")
    else:
        feedback.append("❌ Missing numbers.")

    # Check for Special Characters (Max 2 points)
    # Using 'string.punctuation' for a comprehensive set
    special_chars = string.punctuation
    if any(c in special_chars for c in password):
        score += 2
        feedback.append("✅ Contains special characters (+2 points)")
    else:
        feedback.append("❌ Missing special characters (e.g., !@#$%^&*).")

    # --- 3. Determine Strength Level ---
    if score >= 12:
        strength_level = "🏆 Very Strong"
    elif score >= 9:
        strength_level = "🟢 Strong"
    elif score >= 5:
        strength_level = "🟡 Moderate"
    else:
        strength_level = "🔴 Very Weak"

    return score, strength_level, feedback

def main():
    """Main function to run the password checker tool."""
    print("\n--- Password Complexity Checker Tool ---")
    password = input("Enter a password to check its strength: ")
    
    if not password:
        print("Password cannot be empty.")
        return

    total_score, strength, report = check_password_strength(password)
    
    print("\n" + "="*40)
    print(f"| Strength Level: {strength.ljust(22)}|")
    print(f"| Total Score: {str(total_score).ljust(25)}/ 13 |")
    print("="*40)
    
    print("\nDetailed Feedback:")
    for item in report:
        print(f"- {item}")
    
    print("\n--- Summary ---")
    if "Very Weak" in strength or "Moderate" in strength:
        print("Recommendation: Review the missing criteria (❌ items) to improve your password security.")
    elif "Strong" in strength:
        print("Excellent! This password offers good security.")
    else:
        print("Perfect score! This password is highly secure.")

if __name__ == '__main__':
    main()
