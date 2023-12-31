# Find occurrences of a needle image in a haystack image using multiprocessing and template matching.

## pip install needlefinder

### Tested against Python 3.11 / Windows 10


```python

# For different needle sizes 

from needlefinder import find_needle_in_haystack

INTER_NEAREST = 0
INTER_LINEAR = 1
INTER_CUBIC = 2
INTER_AREA = 3
INTER_LANCZOS4 = 4
INTER_LINEAR_EXACT = 5
INTER_NEAREST_EXACT = 6
INTER_MAX = 7
WARP_FILL_OUTLIERS = 8
WARP_INVERSE_MAP = 16
interpolation = INTER_AREA
haystack = r"C:\savedsch\1349.png" # accepts almost everything - np.arrays / PIL / base64 / bytes ...
needle = r"C:\savedsch\1313.png"
df = find_needle_in_haystack(
    haystack,
    needle,
    with_image_data=True,
    percentage_min=80,
    percentage_max=120,
    steps=3,
    thresh=0.85,
    interpolation=interpolation,
    pad_input=False,
    mode="constant",
    constant_values=0,
    needlename="arrow",
    usecache=True,
    processes=3,
    chunks=1,
    print_stdout=False,
    print_stderr=True,
)
print(df)

Parameters:
- haystack (str): Path to the haystack image file.
- needle (str): Path to the needle image file.
- with_image_data (bool, optional): If True, include image data in the result DataFrame. Default is True.
- percentage_min (int, optional): Minimum scale percentage for resizing the needle image. Default is 50.
- percentage_max (int, optional): Maximum scale percentage for resizing the needle image. Default is 150.
- steps (int, optional): Scale percentage steps for resizing the needle image. Default is 1.
- thresh (float, optional): Threshold for matching. Values below this threshold are considered non-matches. Default is 0.9.
- interpolation (int, optional): Interpolation method for resizing. Default is 4 (cv2.INTER_LANCZOS4).
- pad_input (bool, optional): If True, pad the input image. Default is False.
- mode (str, optional): Padding mode. Default is 'constant'.
- constant_values (int, optional): Constant value for padding. Default is 0.
- needlename (str, optional): Name of the needle used for identification in the result DataFrame. Default is 'arrow'.
- usecache (bool, optional): If True, use caching during multiprocessing. Default is True.
- processes (int, optional): Number of processes to use for multiprocessing. Default is 5.
- chunks (int, optional): Chunk size for multiprocessing. Default is 1.
- print_stdout (bool, optional): If True, print stdout during multiprocessing. Default is False.
- print_stderr (bool, optional): If True, print stderr during multiprocessing. Default is True.

Returns:
- DataFrame: Result DataFrame containing information about matched occurrences.
  Columns include 'aa_start_x', 'aa_start_y', 'aa_scale_factor', 'aa_width', 'aa_height',
  'aa_match', 'aa_end_x', 'aa_end_y', 'aa_center_x', 'aa_center_y', 'aa_area', 'aa_needlename'.
  If with_image_data is True, additional columns include 'aa_screenshot', 'aa_r', 'aa_g', 'aa_b', 'aa_old_index'.


# for multiple needles and multiple haystacks 

needles = {
    "n1": r"C:\screeeni\16.png",
    "n2": r"C:\screeeni\123.png",
    "n3": r"C:\screeeni\130.png",
}

haystacks =[
r"C:\haystacks\161.png",
r"C:\haystacks\523.png",
r"C:\haystacks\630.png",]

INTER_NEAREST = 0
INTER_LINEAR = 1
INTER_CUBIC = 2
INTER_AREA = 3
INTER_LANCZOS4 = 4
INTER_LINEAR_EXACT = 5
INTER_NEAREST_EXACT = 6
INTER_MAX = 7
WARP_FILL_OUTLIERS = 8
WARP_INVERSE_MAP = 16
interpolation = INTER_AREA
dfneedles = find_needles_in_multi_haystacks(
    haystacks=haystacks,
    needles=needles,
    with_image_data=True,
    thresh=0.9,
    pad_input=False,
    mode="constant",
    constant_values=0,
    usecache=True,
    processes=5,
    chunks=1,
    print_stdout=False,
    print_stderr=True,
)

Find occurrences of multiple needle images in multiple haystack images using template matching.

Parameters:
- haystacks (List[str]): List of haystack images.
- needles (Dict[str, str]): Dictionary where keys are needle names and values are the needle images.
- with_image_data (bool, optional): If True, include image data in the result DataFrame. Default is True.
- thresh (float, optional): Threshold for matching. Values below this threshold are considered non-matches. Default is 0.9.
- pad_input (bool, optional): If True, pad the input image. Default is False.
- mode (str, optional): Padding mode. Default is 'constant'.
- constant_values (int, optional): Constant value for padding. Default is 0.
- usecache (bool, optional): If True, use caching during multiprocessing. Default is True.
- processes (int, optional): Number of processes to use for multiprocessing. Default is 5.
- chunks (int, optional): Chunk size for multiprocessing. Default is 1.
- print_stdout (bool, optional): If True, print stdout during multiprocessing. Default is False.
- print_stderr (bool, optional): If True, print stderr during multiprocessing. Default is True.

Returns:
- DataFrame: Result DataFrame containing information about matched occurrences.
  Columns include 'aa_start_x', 'aa_start_y', 'aa_scale_factor', 'aa_width', 'aa_height',
  'aa_match', 'aa_end_x', 'aa_end_y', 'aa_center_x', 'aa_center_y', 'aa_area', 'aa_needlename',
  'aa_img_index'.
  If with_image_data is True, additional columns include 'aa_screenshot', 'aa_r', 'aa_g', 'aa_b'.

Raises:
- Exception: If an error occurs during multiprocessing or matching.

Notes:
- The 'aa_old_index' column is retained from individual matches.

```