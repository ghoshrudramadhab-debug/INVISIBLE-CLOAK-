import cv2

# THE CAPTURED BACKGROUND IMAGE 
image_path= r"C:\Users\RUDRA MADHAB GHOSH\OneDrive\Pictures\Camera Roll 1\WIN_20260130_20_55_50_Pro.jpg"
read_image= cv2.imread(image_path)

# SETTING THE WEBCAM 
cap= cv2.VideoCapture(0)

# HSV COLOR RANGE FOR BLUE: 
hsv_ranges_blue = [90, 80, 80, 130, 255, 255]



while True:
    ret_urn, frame= cap.read()

    hsv_frame= cv2.cvtColor(frame, cv2.COLOR_BGR2HSV)

    from hsv_limit_function import get_limits_adv

    lower, upper = get_limits_adv(hsv_ranges_blue)
    mask= cv2.inRange(hsv_frame,lower, upper)

    # CHECK FOR THE REGION CO-ODINATES WHERE YELLOW IS DETECTED
    from PIL import Image

    mask_pil= Image.fromarray(mask)
    bbox= mask_pil.getbbox()

    if bbox: 

       x1,y1,x2,y2=  bbox
       bg_roi = read_image[y1:y2, x1:x2]
       frame[y1:y2, x1:x2] = bg_roi
      

    
    cv2.imshow("Invisible Effect", frame)

    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

cap.release()
cv2.destroyAllWindows()







